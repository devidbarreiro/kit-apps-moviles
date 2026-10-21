# Tiendas: lo que hemos aprendido

Cifras y límites cambian: compruébalos en la ayuda oficial de App Store Connect y Play Console antes de dárselos a la persona.

## Cuentas y costes

| Cosa | Coste | Notas |
|---|---|---|
| Apple Developer Program | 99 €/año | Sin él: ni TestFlight, ni development build en iPhone, ni publicar. Si no se renueva, la app desaparece de la tienda. |
| Google Play Console | 25 $ una vez | Verificación de identidad y de un móvil Android. |
| EAS (Expo) | Gratis con cupo mensual | El cupo de iOS se agota pronto; `eas build --local` (Mac) no lo gasta. |
| Backend (Supabase) | Gratis para empezar | Se pausa sin actividad; para una app publicada, valora el plan de pago. |
| Web de privacidad/soporte | 0 € | GitHub Pages, Vercel, Cloudflare Pages… |
| Comisión de las tiendas | 15 % (programas para pequeños desarrolladores) / 30 % | Solo si cobras por contenido digital. |

- **Persona o empresa**: como persona, tu nombre real aparece como vendedor. Como empresa hace falta D-U-N-S (gratis, tarda).
- **Google Play, cuenta personal nueva**: prueba cerrada con **≥12 testers durante 14 días seguidos** antes de producción.
- **UE (DSA)**: hay que declarar si eres comerciante. Una de nuestras apps salió aprobada pero **no aparecía en ninguna tienda de la UE** hasta declararlo. «No comerciante» solo encaja con apps gratuitas sin actividad comercial; si cobras, tu dirección y teléfono serán públicos.
- **Bundle id / package**: permanente una vez creada la app en la consola. Renombra antes.
- **Nombre**: único en cada tienda. Si App Store Connect añade un sufijo raro, está cogido. Búscalo antes de diseñar la marca.
- **Gratis → de pago** no se puede en Google Play. Compras integradas sí.

## Flujo que nos funciona

1. Development build pronto (antes del primer módulo nativo).
2. TestFlight interno y prueba interna de Google en cuanto el MVP se usa de punta a punta.
3. Ficha + «Contenido de la app» completos → prueba cerrada de Google (14 días) en paralelo a los últimos arreglos.
4. Revisión de Apple con **publicación manual**.
5. Mientras está en revisión: congelar funcionalidades.
6. Después, arreglos de JS por EAS Update; cambios nativos con versión nueva.

Tiempos vistos: Apple suele contestar en 1-2 días (la primera vez, más). Del primer build en TestFlight a la 1.0 publicada: de días a unas semanas. Una app con varios rechazos tardó cinco semanas. Google: la primera revisión de la prueba cerrada y la solicitud de producción, varios días cada una.

## Recursos gráficos

- **Icono**: Apple 1024×1024 **sin** transparencia; Google 512×512 (PNG de 32 bits) y icono adaptativo con capa monocroma.
- **Capturas de iPhone**: App Store Connect pide el tamaño grande actual (p. ej. 6,9"); a veces también 6,5". Los simuladores dan tamaños exactos: elige el modelo cuyo tamaño nativo coincide (no todos valen). Mín. 3, máx. 10. Cambiarlas no requiere build.
- `supportsTablet: false` si no está diseñada para iPad: si no, piden capturas de iPad y la revisan en iPad. Tiene efecto solo en un build nuevo.
- **Google**: 2-8 capturas de teléfono (al menos 4 de ≥1080 px para optar a recomendaciones), gráfico destacado 1024×500 sin transparencia.
- **Vídeo de vista previa de Apple**: pista de audio obligatoria (aunque sea silencio), debe entenderse sin sonido, interfaz real.
- **Eventos dentro de la app (Apple)**: nombre ≤30, descripción corta ≤50, larga ≤120; máx. 31 días; imágenes sin texto, logos, marcos ni degradados; rechazan tareas diarias, promociones de precio y «promoción genérica». Se revisan aparte.

## Textos de la ficha

- Nombre (30), subtítulo (30) y palabras clave (100) cuentan para la búsqueda en Apple; no repitas palabras entre ellos. Patrón «Marca: qué hace».
- **Implementa primero, anuncia después**: nada de «IA» si no hay IA, ni «n.º 1», ni «oficial», ni cifras que no puedas demostrar.
- Si la app usa información pública (normativa, horarios, datos de organismos): aviso visible de **app independiente, no vinculada a ninguna administración** en la descripción, en «Acerca de» y donde se muestre esa información, más enlaces a las fuentes oficiales. En el cuestionario: «app gubernamental: no».
- Mientras está en revisión, en redes di «próximamente», no «descárgala ya».
- Categoría: evita «Juegos» si es una app con algo de juego pero no un juego; aumenta el riesgo de rechazo por spam o de categoría.

## Privacidad

- Una sola fuente (`plan/PRIVACIDAD.md`) para la política, las etiquetas de Apple y «Seguridad de los datos». Que todo cuadre con el código, los permisos y los textos de marketing.
- **Declarar de más no penaliza; declarar de menos, sí.** Incluye lo que se envía aunque no se guarde (p. ej. la ubicación para una búsqueda), los tokens de notificaciones y los identificadores.
- Los proveedores que tratan datos por ti (base de datos, push) no son «compartir con terceros» en Google, pero menciónalos en la política.
- **Manifiesto de privacidad de iOS**: las *required reason APIs* (UserDefaults, marcas de tiempo de archivos, espacio en disco, tiempo de arranque…) se declaran en `ios.privacyManifests` de `app.json`. Expo y muchas librerías ya traen el suyo; revisa la doc.
- **Analítica o informes de errores**: decláralos **antes** de activarlos. Si un mapa u otro SDK envía telemetría, desactívala o declárala.
- **Borrar cuenta**: dentro de la app (ambas tiendas) **y** una URL pública que explique cómo pedirlo (Google).
- **Público objetivo** coherente con la política (si la política dice «no dirigida a menores de 14», no marques 13-15).
- **Clasificación por edades**: honesta. Revísala al añadir chat, comentarios o contenido de usuarios.

## Revisores

- Notas en **inglés**: qué hace, cómo probarlo paso a paso, qué necesita (un segundo móvil, un archivo de ejemplo, una ubicación concreta: el revisor puede estar en otro país).
- **Cuenta demo** creada por el registro normal, sin verificación en dos pasos, sin caducidad y con datos de ejemplo. La contraseña solo en la consola y en un gestor de contraseñas.
- **El backend tiene que estar despierto** el día de la revisión (y siempre).
- Login obligatorio sin motivo es un riesgo (guideline 5.1.1(v)): deja usar lo básico sin cuenta si se puede.

## Rechazos reales (generalizados) y cómo se resolvieron

| Motivo | Qué pasó | Qué hicimos |
|---|---|---|
| **Apple 5.1.1(iv)** | La pantalla previa al permiso de cámara tenía un botón «Dar permiso» | Botón neutro «Continuar», enlace a Ajustes si lo deniega y alternativa manual (escribir el código a mano) |
| **Apple 2.1(a)** (la app falla) | Error al crear una sala online: el backend gratuito se había pausado y ni resolvía DNS | Respuesta admitiendo el fallo, explicando que era el servidor y no el binario, y con 3 acciones concretas (aviso de reconexión, monitorización, tarea diaria que lo mantiene despierto) |
| **Apple 4.3(b)** | Un juego para beber: Apple no acepta esa temática aunque se declare bien | Cambio de concepto completo (motor, textos, web, capturas, clasificación) y build nuevo |
| **Apple 1.4.3** tras el cambio | Quedaban números sin unidad que «sonaban» a tragos, **capturas viejas** en la consola y la respuesta anterior decía «hemos quitado el modo de beber» | Unidades explícitas en todas partes, capturas nuevas y respuesta neutra preguntando qué pantalla lo provocaba. Lección: **tu respuesta puede reactivar el rechazo** |
| **Apple ITMS-90683** | Faltaba el texto de un permiso (movimiento) que una librería de ubicación enlaza aunque no se use | Texto honesto añadido y build nuevo |
| **Apple, revisión ampliada** | Aviso tras varios rechazos | No reenviar sin cambios; responder primero en el Centro de resoluciones o pedir «Meet with Apple» |
| **Google, afirmaciones engañosas** | La ficha parecía de un organismo oficial porque enlazaba normativa | Aviso de «app independiente» en descripción, «Acerca de» y la propia app; fuentes oficiales enlazadas; reenviado en dos días |
| **Apple, permisos especiales** (*entitlements*) | Solicitud rechazada por falta de detalle | Reenviada con quién prueba la app, cómo se comporta si falla y cómo llegan los avisos críticos |

Cómo responder: inglés, corto, verificable (qué cambió y dónde verlo), sin discutir y sin describir lo que la app era antes. Guarda cada rechazo y su respuesta en `plan/RECHAZOS.md`.

## Prueba cerrada de Google: cómo conseguir 12 testers

- Recluta 15-20: en tu entorno o en comunidades del tema de la app (en una de nuestras apps funcionó pedirlo por mensaje directo a gente del nicho en redes, con una historia «llega a Android»).
- Necesitan la cuenta de Google de su Play Store, aceptar desde el enlace e instalar desde Play. Pídeles que no desinstalen.
- Arreglos durante la prueba: no reinician los 14 días.
- Al solicitar producción, Google pregunta qué se probó y qué se cambió: recoge 2-3 opiniones por escrito hacia el día 10.
