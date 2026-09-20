# Feature Specification: Unión y Lealtad a Clan

**Jira Epics / Story Key**: `[DRA-03]` *(Automáticamente sincronizado)*  
**Estado**: Draft  
**Última actualización**: 2026-09-20  

---

## 1. Visión General (Doc Origen)
> **Origen**: Referencia al documento en [`docs/HUs.md`](file:///Users/carlosmontero/Documents/Draw.go/docs/HUs.md#L29-L41)  

Permitir que los jugadores se unan a clanes patrocinados para competir grupalmente, aplicando un período de lealtad obligatoria (cooldown) de 60 días, cambiando su identidad visual en el mapa y regulando el comportamiento de usuarios sin clan ("Freelancers").

---

## 2. Historias de Usuario (User Stories)

### HU-03: Unión y Lealtad a Clan
* **Jira Key**: `[DRA-03]`
* **Como**: jugador
* **Quiero**: unirme a un clan patrocinado
* **Para**: competir en equipo y acceder a recompensas de marca

#### Criterios de Aceptación (Gherkin / Given-When-Then)

- [ ] **Dado que** el usuario ingresa a la sección de clanes  
  **Cuando** consulta la lista de clanes disponibles  
  **Entonces** se muestran los clanes oficiales de su ciudad ordenados por popularidad.

- [ ] **Dado que** el usuario visualiza los clanes en la lista  
  **Cuando** examina cada tarjeta de clan  
  **Entonces** puede observar su nombre, logotipo, premios de la temporada actual y número de miembros activos.

- [ ] **Dado que** el usuario busca un clan en particular  
  **Cuando** escribe en la barra de búsqueda  
  **Entonces** la lista se filtra mostrando las coincidencias correspondientes.

- [ ] **Dado que** el usuario selecciona un clan y presiona "Unirme"  
  **Cuando** intenta confirmar la acción  
  **Entonces** el sistema despliega un modal de advertencia informando que no podrá cambiar de clan durante 60 días.

- [ ] **Dado que** el usuario confirma la unión al clan  
  **Cuando** se procesa la solicitud  
  **Entonces** el color del puntero y la estela del jugador cambian inmediatamente al color corporativo del clan.

- [ ] **Dado que** el usuario se ha unido a un clan  
  **Cuando** revisa su perfil  
  **Entonces** observa un contador regresivo visible con los días y horas restantes para completar el período de lealtad (60 días).

- [ ] **Dado que** el período de lealtad de 60 días está activo  
  **Cuando** el usuario ingresa a la configuración de clan  
  **Entonces** el botón "Abandonar Clan" o "Cambiar Clan" se encuentra deshabilitado.

- [ ] **Dado que** concluye el período de bloqueo de 60 días  
  **Cuando** el contador llega a cero  
  **Entonces** el sistema envía una notificación push notificando que ya puede transferirse a otro clan.

- [ ] **Dado que** un usuario abandona un clan tras cumplir el período de lealtad  
  **Cuando** efectúa la salida  
  **Entonces** el territorio previamente conquistado permanece asignado al clan original (sin efecto retroactivo).

- [ ] **Dado que** el usuario decide no unirse a ningún clan ("Freelancer")  
  **Cuando** captura territorio  
  **Entonces** pinta con un color neutral (ej. gris) y no acumula puntos ni elegibilidad para recompensas patrocinadas.

---

## 3. Especificaciones Técnicas y Reglas de Negocio
* **Regla 1 (Bloqueo de Lealtad / Cooldown)**: 60 días naturales a partir de la fecha/hora exacta de confirmación de unión.
* **Regla 2 (No Retroactividad)**: Los metros cuadrados conquistados pertenecen al clan al momento en que se pintaron. Abandonar el clan no resta puntos al clan ni transfiere territorio.
* **Regla 3 (Jugadores Freelancer)**: Color hexadecimal neutral `#808080` (o equivalente definido en el sistema). Exclusiones de tablas de recompensas patrocinadas.
