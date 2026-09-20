# Feature Specification: Despliegue de Publicidad Geofenced y Geo-etiquetas

**Jira Epics / Story Key**: `[DRA-06]` *(Automáticamente sincronizado)*  
**Estado**: Draft  
**Última actualización**: 2026-09-20  

---

## 1. Visión General (Doc Origen)
> **Origen**: Referencia al documento en [`docs/HUs.md`](file:///Users/carlosmontero/Documents/Draw.go/docs/HUs.md#L69-L81)  

Habilitar la exhibición de anuncios publicitarios estáticos en pantalla para jugadores que ingresen a polígonos comerciales dominados por el clan patrocinador, controlando el límite de impresiones por usuario, e integrar la posibilidad de dejar geo-etiquetas de texto (con filtro de profanidad) en hexágonos capturados.

---

## 2. Historias de Usuario (User Stories)

### HU-06: Despliegue de Publicidad Geofenced y Geo-etiquetas
* **Jira Key**: `[DRA-06]`
* **Como**: negocio patrocinador
* **Quiero**: mostrar mis anuncios estáticos en los territorios que domino
* **Para**: dirigir tráfico peatonal a mis sucursales

#### Criterios de Aceptación (Gherkin / Given-When-Then)

- [ ] **Dado que** un clan B2B accede a su panel web  
  **Cuando** sube un anuncio publicitario  
  **Entonces** el sistema valida que sea un banner estático en formato imagen (PNG/JPG) con un peso máximo de 2 MB (sin soporte para video).

- [ ] **Dado que** el banner publicitario ha sido subido  
  **Cuando** se configura la campaña  
  **Entonces** el sistema asocia dicho banner a un "Polígono Comercial" específico en disputa.

- [ ] **Dado que** los jugadores de un clan capturan hexágonos dentro de un polígono comercial  
  **Cuando** el clan alcanza más del 50% de dominio en los hexágonos del polígono  
  **Entonces** el banner publicitario asociado a esa zona se activa automáticamente.

- [ ] **Dado que** un banner está activo en un polígono comercial  
  **Cuando** un jugador entra físicamente al polígono y abre la aplicación  
  **Entonces** el banner publicitario se despliega en pantalla.

- [ ] **Dado que** un usuario ya ha visto un banner específico en las últimas 24 horas  
  **Cuando** vuelve a ingresar al polígono comercial dominado  
  **Entonces** el sistema omite la visualización del anuncio para no saturar la interfaz (frecuencia máxima: 1 vez cada 24 horas por usuario).

- [ ] **Dado que** un banner es visualizado por un jugador  
  **Cuando** se despliega en la app  
  **Entonces** el sistema registra silenciosamente una "Impresión" en la base de datos para métricas del patrocinador.

- [ ] **Dado que** un banner publicitario se muestra en la pantalla del usuario  
  **Cuando** el usuario desea continuar jugando  
  **Entonces** la interfaz ofrece un botón de cierre ("X") claro y fácil de presionar en la esquina superior derecha.

- [ ] **Dado que** un usuario captura un hexágono  
  **Cuando** interactúa con la opción de dejar un mensaje  
  **Entonces** puede anclar una "Geo-etiqueta" de texto de máximo 140 caracteres a ese hexágono.

- [ ] **Dado que** una Geo-etiqueta ha sido anclada a un hexágono  
  **Cuando** cualquier otro jugador camina físicamente por esa misma cuadra o hexágono  
  **Entonces** la Geo-etiqueta resulta visible y legible en su interfaz.

- [ ] **Dado que** un usuario redacta el texto de una Geo-etiqueta  
  **Cuando** presiona guardar/publicar  
  **Entonces** el texto pasa por un filtro automático de palabras inapropiadas para bloquear profanidad o contenido inadecuado antes de hacerse público.

---

## 3. Especificaciones Técnicas y Reglas de Negocio
* **Regla 1 (Especificación de Banner)**: Formatos permitidos: JPG, PNG. Tamaño máximo: 2 MB. Video no soportado.
* **Regla 2 (Activación por Dominio)**: Dominio estricto `> 50.0%` del número total de hexágonos que componen el polígono comercial demarcado.
* **Regla 3 (Cap de Frecuencia de Banners)**: Máximo 1 impresión por `(User_ID, Campaign_ID)` cada 86,400 segundos (24 horas).
* **Regla 4 (Geo-etiquetas & Moderación)**: Límite de 140 caracteres por Geo-etiqueta. Ejecución de filtro moderador regex/blacklist previo al almacenamiento en BD.
