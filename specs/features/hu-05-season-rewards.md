# Feature Specification: Cierre de Temporada y Distribución de Recompensas

**Jira Epics / Story Key**: `[DRA-05]` *(Automáticamente sincronizado)*  
**Estado**: Draft  
**Última actualización**: 2026-09-20  

---

## 1. Visión General (Doc Origen)
> **Origen**: Referencia al documento en [`docs/HUs.md`](file:///Users/carlosmontero/Documents/Draw.go/docs/HUs.md#L55-L68)  

Automatizar el proceso de finalización de temporada cada 60 días, determinando los clanes ganadores por zonas predefinidas, calculando la elegibilidad de los usuarios (mínimo 5 km válidos), entregando premios Base y VIP en la Billetera digital de los usuarios, gestionando el canje de cupones y reiniciando el mapa para una nueva temporada.

---

## 2. Historias de Usuario (User Stories)

### HU-05: Cierre de Temporada y Distribución de Recompensas
* **Jira Key**: `[DRA-05]`
* **Como**: jugador de un clan ganador
* **Quiero**: recibir mis beneficios proporcionales al final de la temporada
* **Para**: hacer válida mi lealtad a la marca

#### Criterios de Aceptación (Gherkin / Given-When-Then)

- [ ] **Dado que** transcurren 60 días desde el inicio de la temporada  
  **Cuando** el reloj alcanza las 23:59 hrs locales del último día  
  **Entonces** el sistema cierra automáticamente la temporada y congela temporalmente las capturas en el mapa.

- [ ] **Dado que** la temporada ha cerrado  
  **Cuando** el motor de cálculo evalúa las zonas predefinidas (ej. polígonos alrededor de plazas como Galerías o Town Square)  
  **Entonces** computa el total de territorio por clan en dichas zonas.

- [ ] **Dado que** se realiza el conteo final por zona  
  **Cuando** un clan obtiene la mayor cantidad de hexágonos netos bajo su control  
  **Entonces** dicho clan es declarado ganador de la temporada en esa zona.

- [ ] **Dado que** un clan es declarado ganador  
  **Cuando** el sistema procesa los miembros del clan  
  **Entonces** filtra y valida a los usuarios que hayan aportado al menos 5 kilómetros validados durante la temporada.

- [ ] **Dado que** el usuario pertenece al clan ganador y cumple con el mínimo de 5 km válidos  
  **Cuando** finaliza el cálculo de premios  
  **Entonces** recibe automáticamente el "Premio Base" (ej. código QR de descuento) en su Billetera de Premios interna.

- [ ] **Dado que** se determina la lista de contribuyentes del clan ganador  
  **Cuando** el sistema ordena la aportación en m²  
  **Entonces** identifica a los 50 usuarios con mayor área pintada ("Top Contributors").

- [ ] **Dado que** un usuario está dentro de los 50 "Top Contributors" del clan ganador  
  **Cuando** se distribuyen los premios  
  **Entonces** se le deposita adicionalmente un "Premio VIP" con condiciones mejoradas en su Billetera.

- [ ] **Dado que** se genera un premio (Base o VIP)  
  **Cuando** se crea el registro del cupón  
  **Entonces** se asigna un código único inalterable, una fecha de caducidad dinámica y un código de barras canjeable en punto de venta.

- [ ] **Dado que** el usuario o el comercio presentan el cupón en el punto de venta  
  **Cuando** se presiona "Canjear" o se escanea el código  
  **Entonces** el sistema lo marca como "Canjeado", invalidándolo de forma inmediata para usos futuros.

- [ ] **Dado que** se completa la distribución de premios  
  **Cuando** concluye la notificación a clanes perdedores y ganadores  
  **Entonces** el mapa borra todos los colores de hexágonos y los contadores personales de kilómetros aportados a clanes se reinician a cero para iniciar la nueva temporada.

---

## 3. Especificaciones Técnicas y Reglas de Negocio
* **Regla 1 (Periodicidad y Cierre)**: Cada 60 días a las 23:59:59 (hora local de la región). Congelamiento temporal del motor de pings/capturas durante el cálculo (máximo 15 minutos).
* **Regla 2 (Criterios de Elegibilidad)**:
  * Premio Base: Clan ganador de zona + `>= 5.0 km` caminados/validados en la temporada.
  * Premio VIP: Clan ganador de zona + estar dentro de los Top 50 en m² pintados del clan.
* **Regla 3 (Estructura de Cupones)**: Hash único de cupón + timestamp de expiración + barcode/QR renderizable + estado enum (`AVAILABLE`, `REDEEMED`, `EXPIRED`).
* **Regla 4 (Reinicio / Reset de Temporada)**: Eliminación del ownership de hexágonos en el mapa activo y reseteo a 0 del contador `season_km_contributed` por usuario.
