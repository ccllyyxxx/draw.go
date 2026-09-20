# Feature Specification: Captura de Territorio y Reglas de Movimiento (Core & Anti-Cheat)

**Jira Epics / Story Key**: `[DRA-02]` *(Automáticamente sincronizado)*  
**Estado**: Draft  
**Última actualización**: 2026-09-20  

---

## 1. Visión General (Doc Origen)
> **Origen**: Referencia al documento en [`docs/HUs.md`](file:///Users/carlosmontero/Documents/Draw.go/docs/HUs.md#L15-L28)  

Mecanismo central del juego ("Core") que permite pintar el mapa a medida que el jugador camina, controlando la velocidad de desplazamiento mediante un sistema anti-trampas (anti-cheat), detección de ubicación falsa ("Mock Locations") y sincronización por pings para optimizar consumo de batería.

---

## 2. Historias de Usuario (User Stories)

### HU-02: Captura de Territorio y Reglas de Movimiento (Core & Anti-Cheat)
* **Jira Key**: `[DRA-02]`
* **Como**: jugador de la Liga Peatonal
* **Quiero**: que mi color pinte el mapa a medida que camino
* **Para**: expandir mi territorio de forma justa

#### Criterios de Aceptación (Gherkin / Given-When-Then)

- [ ] **Dado que** la aplicación está activa recopilando ubicación  
  **Cuando** el usuario se desplaza  
  **Entonces** la app actualiza la ubicación mediante pings al servidor cada 30 segundos para reducir consumo de batería.

- [ ] **Dado que** el servidor recibe un ping de ubicación  
  **Cuando** procesa la coordenada  
  **Entonces** calcula la velocidad promedio alcanzada desde la posición reportada anteriormente.

- [ ] **Dado que** el usuario se mueve a una velocidad promedio menor o igual a 12 km/h  
  **Cuando** el servidor procesa el ping  
  **Entonces** el hexágono/polígono correspondiente se pinta del color del jugador o clan.

- [ ] **Dado que** el usuario registra una velocidad mayor a 12 km/h y menor o igual a 30 km/h (Liga de Ruedas)  
  **Cuando** se evalúa la zona geográfica  
  **Entonces** se permite la captura si se encuentra en parques o ciclovías locales; de lo contrario, se descarta.

- [ ] **Dado que** el usuario supera los 30 km/h de velocidad promedio (ej. traslado vehicular)  
  **Cuando** se detecta este exceso de velocidad  
  **Entonces** el sistema bloquea automáticamente la captura de territorio durante 5 minutos como medida anti-cheat.

- [ ] **Dado que** la aplicación se ejecuta en Android o iOS  
  **Cuando** se verifica el estado de las APIs del sistema operativo  
  **Entonces** si se detectan ubicaciones simuladas ("Mock Locations" / GPS spoofer), la captura se deniega inmediatamente.

- [ ] **Dado que** el usuario ingresa físicamente a un hexágono vacío  
  **Cuando** permanece al menos 3 segundos dentro de sus límites  
  **Entonces** el hexágono cambia al color del usuario.

- [ ] **Dado que** el usuario ingresa a un hexágono controlado por un rival  
  **Cuando** permanece en él al menos 10 segundos  
  **Entonces** el color del hexágono se sobre-escribe con el color del usuario/clan.

- [ ] **Dado que** un hexágono es conquistado  
  **Cuando** se completa la condición de tiempo dentro del hexágono  
  **Entonces** se refleja instantáneamente en el mapa local del dispositivo y se sincroniza con el servidor en el siguiente ping.

- [ ] **Dado que** el usuario realiza capturas de territorio  
  **Cuando** suma hexágonos  
  **Entonces** el sistema acumula los metros cuadrados conquistados en tiempo real y actualiza la barra de progreso de la sesión.

- [ ] **Dado que** la aplicación se envía a segundo plano (background)  
  **Cuando** el permiso de ubicación en background está otorgado  
  **Entonces** la captura de territorio continúa funcionando sin interrupción.

---

## 3. Especificaciones Técnicas y Reglas de Negocio
* **Regla 1 (Frecuencia de Pings)**: Intervalo de 30 segundos por ping para optimización energética.
* **Regla 2 (Umbrales de Velocidad)**:
  * `<= 12 km/h`: Captura libre (Liga Peatonal).
  * `12 km/h - 30 km/h`: Captura restringida solo a geocercas tipo parques/ciclovías.
  * `> 30 km/h`: Penalty de bloqueo de captura por 5 minutos (300 segundos).
* **Regla 3 (Tiempos de Permanencia en Hexágono)**:
  * Hexágono Vacío: 3 segundos mínimos.
  * Hexágono Rival (Conquista): 10 segundos mínimos.
* **Regla 4 (Anti-Cheat / Mock Location)**: Verificación en cada ping de `isFromMockProvider` (Android) / `CLLocation.isSimulated` (iOS).
