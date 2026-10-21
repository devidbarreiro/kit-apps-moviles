# Checklist de tiendas: <Nombre de la app> v<versión>

_Actualizado: AAAA-MM-DD_ · Marca cada casilla solo cuando esté comprobada.

## Común a las dos
- [ ] Nombre definitivo y libre en ambas tiendas (búscalo antes de enamorarte)
- [ ] Icono 1024×1024 sin transparencia ni esquinas redondeadas (iOS); icono adaptativo con capa monocroma (Android)
- [ ] Pantalla de carga (splash) configurada con `expo-splash-screen`
- [ ] Política de privacidad pública (URL que abre sin login)
- [ ] Página o email de soporte (URL que abre sin login)
- [ ] `plan/PRIVACIDAD.md` al día y coherente con la política, los permisos y la app
- [ ] Textos de permisos concretos y en el idioma de la app; ningún permiso que no se use (revisa los que añaden los plugins: micrófono, audio en segundo plano, movimiento…)
- [ ] Si hay cuentas: borrar cuenta desde la app + cuenta de prueba para revisores
- [ ] Si hay contenido de usuarios: reportar, bloquear y moderar
- [ ] Ningún texto «de programador» visible (nombres de variables, errores en inglés, «TODO», «lorem ipsum»)
- [ ] Ninguna afirmación que no se pueda demostrar («la mejor», «oficial», «100 % preciso», «gratis para siempre», números de usuarios inventados)
- [ ] El backend no se duerme (los planes gratuitos se pausan por inactividad: el revisor vería la app rota)
- [ ] Probada en un móvil real con el build de producción, no solo en desarrollo

## App Store (Apple)
- [ ] Apple Developer Program activo (99 USD/año)
- [ ] App creada en App Store Connect con el bundle identifier definitivo
- [ ] `ios.supportsTablet: false` si no está diseñada para iPad (si no, piden capturas de iPad y la revisan en iPad)
- [ ] `ios.infoPlist.ITSAppUsesNonExemptEncryption: false` (si solo usa HTTPS estándar)
- [ ] Capturas de iPhone en el tamaño que pida App Store Connect hoy (de 1 a 10; recomendable 3 o más)
- [ ] Subtítulo (30 caracteres), descripción, palabras clave (100 caracteres), categoría
- [ ] Etiquetas de privacidad («Privacidad de la app») rellenas desde `plan/PRIVACIDAD.md`
- [ ] Clasificación por edades respondida con honestidad
- [ ] Precio y disponibilidad (países)
- [ ] Notas para el revisor (en inglés): qué hace la app, cómo probarla paso a paso, cuenta demo, qué necesita algo especial (segundo móvil, ubicación concreta…)
- [ ] Probada por TestFlight en un iPhone real
- [ ] Login con Google/Facebook/etc. → también «Iniciar sesión con Apple»

## Google Play
- [ ] Cuenta de Play Console verificada (25 USD una vez; verificación de identidad y de un móvil Android)
- [ ] Si la cuenta es personal y nueva: **prueba cerrada con al menos 12 testers durante 14 días seguidos** antes de poder pedir producción
- [ ] Ficha: nombre (30), descripción breve (80), descripción completa (4000), icono 512×512, gráfico destacado 1024×500, capturas de teléfono (mín. 2)
- [ ] «Seguridad de los datos» rellenado desde `plan/PRIVACIDAD.md`
- [ ] Cuestionario de clasificación de contenido (IARC)
- [ ] Público objetivo y contenido (edades); si incluye menores, requisitos de familias
- [ ] Declaración de anuncios, de acceso a la app (credenciales de prueba) y de permisos sensibles (ubicación en segundo plano, etc.)
- [ ] Ficha sin afirmaciones engañosas: cada promesa de la ficha se ve en la app y en las capturas
- [ ] AAB subido a prueba interna y probado en un Android real

## Envío
- [ ] Versión (`version`) subida respecto a la anterior; el número de build lo sube EAS
- [ ] Novedades de la versión escritas
- [ ] Nada nuevo en desarrollo hasta que pase la revisión (congelar)
