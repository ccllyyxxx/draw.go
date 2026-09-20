# Feature Specification: Panel de Métricas Personales y de Clan (Dashboard)

**Jira Epics / Story Key**: `[DRA-04]` *(Automáticamente sincronizado)*  
**Estado**: Draft  
**Última actualización**: 2026-09-20  

---

## 1. Visión General (Doc Origen)
> **Origen**: Referencia al documento en [`docs/HUs.md`](file:///Users/carlosmontero/Documents/Draw.go/docs/HUs.md#L42-L54)  

Proporcionar a los jugadores competitivos un tablero de métricas (dashboard) con estadísticas individuales, contribución al clan, leaderboard con posición fija resaltada, gráficos comparativos de la ciudad, historial de temporadas y tarjetas exportables para redes sociales.

---

## 2. Historias de Usuario (User Stories)

### HU-04: Panel de Métricas Personales y de Clan (Dashboard)
* **Jira Key**: `[DRA-04]`
* **Como**: jugador competitivo
* **Quiero**: ver mis estadísticas individuales y del clan
* **Para**: conocer mi impacto en el juego

#### Criterios de Aceptación (Gherkin / Given-When-Then)

- [ ] **Dado que** el usuario ingresa al panel de estadísticas  
  **Cuando** carga la vista principal del dashboard  
  **Entonces** se muestran los kilómetros totales validados (caminados/aprobados por anti-cheat) en la temporada actual.

- [ ] **Dado que** el usuario consulta su resumen de avance  
  **Cuando** observa las métricas territoriales  
  **Entonces** se despliega el área total en metros cuadrados o número de hexágonos pintados durante la temporada.

- [ ] **Dado que** el usuario pertenece a un clan  
  **Cuando** revisa la sección de aportación  
  **Entonces** el dashboard calcula y muestra el porcentaje exacto de contribución respecto al territorio total del clan.

- [ ] **Dado que** el usuario abre la pestaña del Leaderboard del clan  
  **Cuando** se renderiza la tabla de posiciones  
  **Entonces** se muestra el ranking de los 50 miembros con mayor aportación territorial.

- [ ] **Dado que** el usuario consulta el Leaderboard del clan  
  **Cuando** su posición no está entre los primeros lugares visible en scroll corto  
  **Entonces** una barra flotante inferior resalta siempre su posición actual (ej. "Estás en la posición #142").

- [ ] **Dado que** el usuario revisa el panel general de la ciudad  
  **Cuando** entra a la vista de clan  
  **Entonces** se despliega un gráfico comparativo del porcentaje de dominio de la ciudad frente a clanes rivales.

- [ ] **Dado que** el usuario desea consultar su rendimiento histórico  
  **Cuando** entra a la pestaña "Historial"  
  **Entonces** puede seleccionar y ver los kilómetros y áreas dominadas en temporadas pasadas.

- [ ] **Dado que** la temporada en curso ofrece un "Premio Base"  
  **Cuando** el usuario revisa su meta de avance  
  **Entonces** el dashboard incluye una barra de progreso indicando la distancia/territorio faltante para asegurar dicho premio.

- [ ] **Dado que** el sistema procesa datos de clasificación  
  **Cuando** se actualiza el Leaderboard  
  **Entonces** el recálculo masivo de posiciones se ejecuta al menos una vez al día para optimizar la base de datos.

- [ ] **Dado que** el usuario desea compartir sus logros  
  **Cuando** presiona el botón "Compartir"  
  **Entonces** se genera una tarjeta gráfica sintetizada lista para exportar a Instagram Stories o WhatsApp.

---

## 3. Especificaciones Técnicas y Reglas de Negocio
* **Regla 1 (Frecuencia de Actualización de Leaderboard)**: Actualización batch (cron job) al menos 1 vez cada 24 horas para evitar sobrecarga en la BD.
* **Regla 2 (Barra Flotante de Posición)**: La barra persiste en el viewport inferior de la pantalla Leaderboard si la fila del usuario no está visible actualmente.
* **Regla 3 (Generación de Tarjeta Social)**: Renderizado dinámico en canvas/imagen nativa con estadísticas clave (Km, m², Clan, Nickname) optimizada para aspecto 9:16 (Stories) o 1:1 (WhatsApp).
