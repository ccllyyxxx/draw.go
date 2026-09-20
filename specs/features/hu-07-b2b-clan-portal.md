# Feature Specification: Creación de Clanes Oficiales B2B (Panel Web)

**Jira Epics / Story Key**: `[DRA-07]` *(Automáticamente sincronizado)*  
**Estado**: Draft  
**Última actualización**: 2026-09-20  

---

## 1. Visión General (Doc Origen)
> **Origen**: Referencia al documento en [`docs/HUs.md`](file:///Users/carlosmontero/Documents/Draw.go/docs/HUs.md#L82-L94)  

Proporcionar un portal web administrativo para que los negocios y marcas patrocinadoras puedan registrarse, verificar su legitimidad, seleccionar su color oficial con validación de contraste/similitud, configurar premios y campañas, realizar el pago de suscripción vía Stripe y monitorear en tiempo real el rendimiento de sus clanes.

---

## 2. Historias de Usuario (User Stories)

### HU-07: Creación de Clanes Oficiales B2B (Panel Web)
* **Jira Key**: `[DRA-07]`
* **Como**: administrador de un negocio
* **Quiero**: registrar y configurar mi clan oficial en un portal web
* **Para**: comenzar a competir y atraer usuarios en la aplicación

#### Criterios de Aceptación (Gherkin / Given-When-Then)

- [ ] **Dado que** un representante de negocio accede al portal web B2B  
  **Cuando** inicia el proceso de registro  
  **Entonces** el sistema requiere y valida la legitimidad mediante un correo de dominio corporativo o datos de identificación fiscal (RFC).

- [ ] **Dado que** el negocio se ha registrado exitosamente  
  **Cuando** configura el perfil del clan  
  **Entonces** el sistema exige la carga de un logotipo oficial en formato PNG con transparencia obligatoria.

- [ ] **Dado que** el administrador configura la apariencia del clan  
  **Cuando** interactúa con el selector de color  
  **Entonces** puede escoger el "Color Hexadecimal" primario que representará al clan en los mapas de la app.

- [ ] **Dado que** el administrador selecciona un color hexadecimal  
  **Cuando** intenta confirmarlo  
  **Entonces** el sistema valida en tiempo real que dicho color no sea idéntico o sumamente similar a otro clan activo en la misma región geográfica.

- [ ] **Dado que** el administrador ingresa a la sección de premios  
  **Cuando** configura la campaña de la temporada  
  **Entonces** puede redactar la oferta detallada para el Premio Base y Premio VIP, además de cargar los lotes de códigos de cupones a distribuir.

- [ ] **Dado que** el administrador concluye la configuración inicial  
  **Cuando** procede al alta del clan  
  **Entonces** se integra con Stripe para procesar la suscripción mensual requerida para mantener activo el clan oficial.

- [ ] **Dado que** el clan está activo  
  **Cuando** el administrador entra al dashboard del portal web  
  **Entonces** puede visualizar un mapa en vivo (solo lectura) con el territorio actualmente dominado por el color de su negocio.

- [ ] **Dado que** el administrador navega en el panel de analíticas  
  **Cuando** revisa el resumen ejecutivo  
  **Entonces** se despliegan métricas globales como la cantidad total de jugadores suscritos al clan y la superficie urbana bajo su control (en m²).

- [ ] **Dado que** el clan desea actualizar su material promocional  
  **Cuando** el administrador sube un nuevo banner publicitario  
  **Entonces** el cambio se aplica de inmediato sin alterar ni reiniciar el estado de los territorios conquistados.

- [ ] **Dado que** se crea la campaña publicitaria o de clan  
  **Cuando** el administrador define el alcance  
  **Entonces** puede marcar la campaña como "Nacional" o como "Local" (limitada a una zona geográfica o municipio específico).

---

## 3. Especificaciones Técnicas y Reglas de Negocio
* **Regla 1 (Verificación Empresarial)**: Validación sintáctica y de registro DNS/MX del correo corporativo (rechazando webmails comunes como gmail/hotmail) o validación de RFC/tax ID.
* **Regla 2 (Logotipo PNG Transparent)**: Formato PNG obligatorio con canal alfa (transparencia).
* **Regla 3 (Algoritmo de Similitud de Color)**: Validación mediante fórmula de diferencia de color Delta E (CIE76 / CIEDE2000). Si `Delta E < THRESHOLD` respecto a un clan rival activo en la misma región, se rechaza la selección del color.
* **Regla 4 (Integración Stripe)**: Stripe Checkout / Customer Portal para gestión de cobro recurrente mensual (SaaS B2B). Si la suscripción se cancela o incurre en impago, el clan pasa a estado `SUSPENDED`.
