# Feature Specification: Registro e Inicio de sesión (Auth & Onboarding)

**Jira Epics / Story Key**: `[DRA-01]` *(Automáticamente sincronizado)*  
**Estado**: Draft  
**Última actualización**: 2026-09-20  

---

## 1. Visión General (Doc Origen)
> **Origen**: Referencia al documento en [`docs/HUs.md`](file:///Users/carlosmontero/Documents/Draw.go/docs/HUs.md#L2-L14)  

Permitir que un usuario nuevo o existente pueda registrarse e iniciar sesión de forma rápida y segura a través de autenticación social (Google OAuth, Apple ID) o mediante correo y contraseña con verificación, gestionando la asignación de nickname único, aceptación de términos y otorgamiento de permisos clave (ubicación y notificaciones).

---

## 2. Historias de Usuario (User Stories)

### HU-01: Registro e Inicio de sesión (Auth & Onboarding)
* **Jira Key**: `[DRA-01]`
* **Como**: usuario nuevo
* **Quiero**: poder registrarme de forma rápida y segura
* **Para**: guardar mi progreso y estadísticas de juego

#### Criterios de Aceptación (Gherkin / Given-When-Then)

- [ ] **Dado que** el usuario elige registrarse mediante Google OAuth  
  **Cuando** se completa la autenticación externa en Google exitosamente  
  **Entonces** el sistema inicia sesión o crea la cuenta asociada al usuario.

- [ ] **Dado que** el usuario elige registrarse mediante Apple ID  
  **Cuando** se completa la autenticación externa en Apple exitosamente  
  **Entonces** el sistema inicia sesión o crea la cuenta asociada al usuario.

- [ ] **Dado que** el usuario desea registrarse mediante correo electrónico y contraseña  
  **Cuando** ingresa una contraseña válida (mínimo 8 caracteres, 1 mayúscula y 1 número)  
  **Entonces** el sistema permite la creación de la cuenta y envía un correo de confirmación.

- [ ] **Dado que** el usuario se ha registrado mediante correo y contraseña  
  **Cuando** intenta iniciar sesión por primera vez sin haber verificado su correo  
  **Entonces** el sistema exige la confirmación previa a través del link de verificación.

- [ ] **Dado que** el usuario realiza su primer inicio de sesión  
  **Cuando** accede a la app  
  **Entonces** el sistema presenta una pantalla explicativa y solicita permisos de ubicación en primer y segundo plano (Foreground y Background).

- [ ] **Dado que** el usuario está en la pantalla de solicitud de permisos de ubicación  
  **Cuando** rechaza otorgar dichos permisos  
  **Entonces** la app muestra un mensaje bloqueante explicando que los permisos de ubicación son indispensables para la captura de territorio.

- [ ] **Dado que** el usuario está en el proceso de registro / onboarding  
  **Cuando** interactúa con el formulario  
  **Entonces** se debe requerir la aceptación explícita de los Términos y Condiciones y la Política de Privacidad.

- [ ] **Dado que** el usuario está configurando su perfil inicial  
  **Cuando** ingresa un "Nickname" alfanumérico de entre 3 y 15 caracteres  
  **Entonces** el sistema valida que sea único y no esté en uso por otro jugador.

- [ ] **Dado que** el usuario completa el flujo inicial de registro  
  **Cuando** avanza en la configuración  
  **Entonces** el sistema solicita permiso opcional para enviar notificaciones push.

- [ ] **Dado que** el usuario inicia sesión exitosamente  
  **Cuando** navega en la aplicación  
  **Entonces** la sesión se mantiene activa mediante un token con vigencia de 30 días o hasta que el usuario cierre sesión manualmente.

---

## 3. Especificaciones Técnicas y Reglas de Negocio
* **Regla 1 (Validación de contraseña)**: La contraseña debe tener como mínimo 8 caracteres, al menos 1 letra mayúscula y al menos 1 número.
* **Regla 2 (Nickname Único)**: Solo se permiten caracteres alfanuméricos (`a-z`, `A-Z`, `0-9`), con una longitud de 3 a 15 caracteres. La consulta de disponibilidad debe ser en tiempo real.
* **Regla 3 (Ubicación Bloqueante)**: Sin permisos de ubicación (Foreground y Background) no se permite el ingreso a la vista principal del juego/mapa.
* **Regla 4 (Expiración de Token)**: Los JWT u OAuth tokens de sesión deben tener una duración de 30 días de inactividad máxima.
