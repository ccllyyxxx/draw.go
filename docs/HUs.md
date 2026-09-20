# Historias de Usuario (HU) - Proyecto draw.go
## HU-01: Registro e Inicio de sesión (Auth & Onboarding)
Como usuario nuevo, quiero poder registrarme de forma rápida y segura para guardar mi progreso y estadísticas de juego.
### Criterios de Aceptación:
El usuario puede iniciar sesión mediante Google OAuth.
El usuario puede iniciar sesión mediante Apple ID.
El usuario puede registrarse usando un correo electrónico y contraseña (mínimo 8 caracteres, 1 mayúscula, 1 número).
El sistema debe solicitar confirmación de correo electrónico mediante un link de verificación antes del primer inicio.
Durante el primer inicio de sesión, la app debe solicitar permisos de ubicación (Foreground y Background) con una pantalla explicativa.
Si el usuario rechaza los permisos de ubicación, la app muestra un mensaje bloqueante explicando que son indispensables para la captura de territorio.
El sistema debe solicitar la aceptación de los Términos y Condiciones y la Política de Privacidad de forma explícita.
El usuario debe poder elegir un "Nickname" único (alfanumérico, 3-15 caracteres) que no esté actualmente en uso por otro jugador.
Se debe solicitar permiso opcional para enviar notificaciones push.
La sesión del usuario debe mantenerse activa mediante un token que expira en 30 días o al cerrar sesión manualmente.
## HU-02: Captura de Territorio y Reglas de Movimiento (Core & Anti-Cheat)
Como jugador de la Liga Peatonal, quiero que mi color pinte el mapa a medida que camino para expandir mi territorio de forma justa.
### Criterios de Aceptación:
La app debe actualizar la ubicación del usuario mediante "pings" al servidor cada 30 segundos en lugar de tiempo real continuo para ahorrar batería.
Cada ping debe calcular la velocidad promedio del usuario desde el punto de ubicación anterior.
Si la velocidad es menor o igual a 12 km/h, el polígono/hexágono correspondiente se pinta del color del jugador o clan.
Si la velocidad es mayor a 12 km/h y menor a 30 km/h (Liga de Ruedas), y el usuario está en una zona permitida (parques o ciclovías locales), el área se captura; de lo contrario, se descarta.
Si la velocidad supera los 30 km/h (ej. auto por Tollocan), el sistema bloquea automáticamente la captura de territorio durante 5 minutos para evitar trampas vehiculares.
El sistema debe verificar banderas del sistema operativo ("Mock Locations") en Android/iOS; si detecta GPS simulado, la captura es denegada inmediatamente.
Al entrar físicamente a un hexágono vacío, este debe cambiar al color del usuario tras permanecer al menos 3 segundos dentro de sus límites.
Al entrar a un hexágono dominado por un rival, el usuario debe permanecer en él al menos 10 segundos para "sobre-escribir" el color.
El área capturada se debe reflejar visualmente en el mapa local del celular al instante, y sincronizarse con el servidor central en el siguiente ping.
El sistema debe sumar los metros cuadrados capturados en tiempo real y actualizar la barra de progreso de la sesión actual.
Cuando el usuario manda la aplicación a segundo plano (background), la captura debe continuar si se otorgó el permiso adecuado.
## HU-03: Unión y Lealtad a Clan
Como jugador, quiero unirme a un clan patrocinado para competir en equipo y acceder a recompensas de marca.
### Criterios de Aceptación:
El usuario debe poder ver una lista de clanes oficiales disponibles en su ciudad, ordenados por popularidad.
Cada clan listado debe mostrar su nombre, logotipo, premios de la temporada actual y la cantidad de miembros activos.
El usuario puede utilizar una barra de búsqueda para encontrar un clan en específico.
Antes de confirmar la unión a un clan, el sistema debe mostrar un modal de advertencia indicando que no podrá cambiar de bando por 60 días.
Al aceptar unirse, el color del puntero y la estela del jugador cambian inmediatamente al color corporativo del clan.
Se inicia un contador regresivo visible en el perfil del usuario mostrando los días y horas restantes para que expire el bloqueo de lealtad (cooldown).
La interfaz debe deshabilitar el botón "Abandonar Clan" o "Cambiar Clan" mientras el periodo de bloqueo de 60 días esté activo.
Una vez expirado el bloqueo, el usuario recibe una notificación push informándole que es libre de transferirse a otro clan.
El territorio previamente conquistado por el usuario se queda registrado a favor del clan original, no es retroactivo al momento de abandonar la facción.
Un usuario que decide no unirse a ningún clan ("Freelancer") pinta con un color neutral (ej. gris) y no acumula puntos para recompensas patrocinadas.
## HU-04: Panel de Métricas Personales y de Clan (Dashboard)
Como jugador competitivo, quiero ver mis estadísticas individuales y del clan para conocer mi impacto en el juego.
### Criterios de Aceptación:
El panel debe mostrar los kilómetros totales validados (caminados/validados por anti-cheat) del usuario en la temporada actual.
El panel debe mostrar el área total (en metros cuadrados o cantidad de hexágonos) pintada por el usuario en la temporada.
El dashboard debe calcular y mostrar el porcentaje exacto de contribución del usuario respecto al total dominado por su clan.
Debe existir un Leaderboard global del clan, mostrando el ranking de los 50 usuarios con mayor aportación territorial.
El Leaderboard debe incluir una barra flotante que resalte siempre la posición actual del usuario ("Estás en la posición #142").
El panel del Clan debe mostrar un gráfico comparativo del porcentaje de dominio de la ciudad en relación con los clanes rivales.
Debe existir una pestaña de "Historial" donde el usuario pueda consultar sus kilómetros y áreas dominadas en temporadas pasadas.
El dashboard debe incluir una barra de progreso que indique cuánto territorio o kilómetros faltan para asegurar el "Premio Base".
Las métricas de Leaderboard se actualizarán masivamente al menos una vez al día para optimizar consultas a la base de datos.
La interfaz debe incluir un botón de "Compartir" que genere una tarjeta gráfica con el resumen de estadísticas para exportar a Instagram Stories o WhatsApp.
## HU-05: Cierre de Temporada y Distribución de Recompensas
Como jugador de un clan ganador, quiero recibir mis beneficios proporcionales al final de la temporada para hacer válida mi lealtad a la marca.
### Criterios de Aceptación:
El sistema debe cerrar la temporada automáticamente cada 60 días a las 23:59 hrs locales, congelando temporalmente el mapa.
El motor calcula el total de territorio por clan en zonas predefinidas (ej. polígonos alrededor de plazas como Galerías o Town Square) para determinar al ganador.
El clan con mayor cantidad de hexágonos netos bajo su control se declara el ganador de la temporada en esa zona.
El sistema filtra a los usuarios del clan ganador y verifica que hayan aportado al menos 5 kilómetros válidos.
Los usuarios elegibles reciben el "Premio Base" (ej. un código QR de descuento) en su Billetera de Premios interna.
El sistema identifica a los 50 usuarios de ese clan que más área pintaron ("Top Contributors").
A los Top Contributors se les deposita un "Premio VIP" independiente, con condiciones mejoradas, en su Billetera.
Los cupones generados deben incluir un código único inalterable, fecha de caducidad dinámica y un código de barras canjeable en el punto de venta.
El usuario o el negocio debe poder marcar el cupón como "Canjeado", invalidándolo inmediatamente para usos futuros en el servidor.
El sistema notifica a los clanes perdedores y, exactamente tras el cálculo y entrega de premios, borra todos los colores del mapa para reiniciar la guerra urbana (Temporada nueva).
Los contadores personales de kilómetros aportados a clanes se reinician a cero.
## HU-06: Despliegue de Publicidad Geofenced y Geo-etiquetas
Como negocio patrocinador, quiero mostrar mis anuncios estáticos en los territorios que domino para dirigir tráfico peatonal a mis sucursales.
### Criterios de Aceptación:
Los clanes B2B deben poder subir una imagen publicitaria (banner estático) de máximo 2 MB de peso desde su panel de control web (sin soporte para video).
El sistema asocia este banner a un "Polígono Comercial" específico disputado.
Cuando el clan logra dominar más del 50% de los hexágonos de dicho polígono, su banner publicitario se activa.
Cuando un jugador entra físicamente a un polígono dominado y abre la aplicación, el banner se despliega en pantalla.
El anuncio publicitario se mostrará un máximo de una vez por usuario cada 24 horas para evitar saturación de la interfaz.
El sistema debe registrar silenciosamente una "Impresión" en la base de datos cada vez que un usuario visualiza el banner.
Todo banner debe tener un botón de cierre ("X") fácil de tapar en la esquina superior derecha.
Los usuarios deben poder dejar "Geo-etiquetas" (mensajes de texto de máximo 140 caracteres) ancladas a hexágonos que ellos mismos capturaron.
Las Geo-etiquetas deben ser legibles para cualquier jugador que camine físicamente por esa misma cuadra o hexágono.
El texto de las Geo-etiquetas de usuarios debe pasar por un filtro automático (lista negra de palabras) para bloquear profanidad o contenido inapropiado.
## HU-07: Creación de Clanes Oficiales B2B (Panel Web)
Como administrador de un negocio, quiero registrar y configurar mi clan oficial en un portal web para comenzar a competir y atraer usuarios en la aplicación.
### Criterios de Aceptación:
El negocio debe registrarse mediante un portal web que valide el dominio de la empresa (correo corporativo) o información fiscal (RFC) para verificar legitimidad.
El negocio debe poder cargar un logotipo oficial (formato PNG con transparencia obligatoria).
El portal web debe permitir seleccionar el "Color Hexadecimal" primario que representará al clan en el mapa de los usuarios.
El sistema debe validar en tiempo real que el color escogido no sea idéntico o sumamente similar a otro clan activo en la misma región.
El administrador debe poder redactar la oferta de sus premios (Premio Base y Premio VIP) y subir los lotes de códigos a distribuir al final de la temporada.
Se debe integrar una pasarela de pagos B2B (Stripe) para gestionar la suscripción mensual del clan oficial.
El panel debe incluir un mapa en vivo (solo de lectura) que muestre el territorio actualmente pintado con el color del negocio.
El panel debe mostrar métricas globales: cantidad total de jugadores suscritos al clan y área urbana bajo su control en metros cuadrados.
El administrador debe poder actualizar el banner publicitario activo en cualquier momento sin afectar el estado de los territorios conquistados.
El administrador debe poder definir si su campaña y clan es de alcance Nacional o de alcance Local (limitado a una zona geográfica específica).
