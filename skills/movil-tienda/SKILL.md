---
name: movil-tienda
description: Prepara una app móvil para App Store y Google Play - cuentas de desarrollador, nombre, icono, capturas, textos de la ficha, política de privacidad y web de soporte, etiquetas de privacidad de Apple, "Seguridad de los datos" y clasificación de contenido de Google, cuenta demo para revisores, estado de comerciante (DSA) en la UE y la prueba cerrada de 12 testers y 14 días. Úsala cuando digan "preparar la ficha", "capturas para la tienda", "política de privacidad", "data safety", "prueba cerrada", "testers de Google", o `/movil-tienda`.
---

# movil-tienda

> **Archivos compartidos**: `REGLAS.md`, `plantillas/` y `referencias/` están en la skill `movil`, carpeta hermana de esta (`../movil/`). Si no la encuentras ahí, búscala en `~/.claude/skills/movil/` o en `~/.claude/plugins/` (`**/movil/REGLAS.md`). Si en el proyecto no existe `plan/ESTADO.md`, propón empezar por la skill `movil`.

Lee `REGLAS.md` y `referencias/tiendas.md` de la skill `movil`, `plan/PRIVACIDAD.md` y `plan/ALCANCE.md`. Copia `plantillas/CHECKLIST_TIENDAS.md` a `plan/CHECKLIST_TIENDAS.md` y ve marcando. Comprueba los requisitos actuales en la ayuda oficial de App Store Connect y de Play Console antes de dar tamaños o límites.

## Orden (de lo que más tarda a lo que menos)

### 1. Cuentas (empieza ya: tardan días)
- **Apple Developer Program** (99 €/año): https://developer.apple.com/programs/enroll/. Como persona: tu nombre real saldrá como vendedor. Como empresa: necesita número D-U-N-S.
- **Google Play Console** (25 $ una vez): https://play.google.com/console/signup. Verificación de identidad y de un móvil Android. **Cuenta personal nueva → prueba cerrada obligatoria** (punto 6).
- **Acuerdos y datos fiscales/bancarios**: solo obligatorios si cobra (app de pago o compras integradas). Si es gratis, no hacen falta para publicar.
- **UE (DSA)**: ambas tiendas piden declarar si eres **comerciante** («trader»). Si no se declara, la app puede **no aparecer en las tiendas de la UE** aunque esté aprobada. Si cobra o es actividad profesional, es comerciante y su dirección y teléfono serán públicos en la ficha. Decídelo antes del lanzamiento (consulta la ayuda de cada tienda).

### 2. Nombre e identidad
- Comprueba que el nombre está libre en las dos tiendas (y el dominio si quiere web). Si App Store Connect añade un sufijo raro al nombre, está cogido.
- Nombre de tienda (30 caracteres) ≠ nombre bajo el icono (corto). Patrón útil: «Marca: qué hace» (p. ej. «Brotes: diario de plantas»).
- Crea la app en App Store Connect y en Play Console con el bundle id / package de `app.json` (no se pueden cambiar después).

### 3. Web de privacidad y soporte
- Rellena `plantillas/POLITICA_PRIVACIDAD.md` desde `plan/PRIVACIDAD.md`. Avísale de que es orientativa, no asesoramiento legal.
- Publica dos páginas públicas (sin login): **privacidad** y **soporte** (con email de contacto). Una web estática gratuita basta (GitHub Pages, Vercel, Cloudflare Pages, Netlify). Si hay cuentas, añade una sección o URL de **cómo borrar la cuenta** (Google la pide aparte).
- Cada afirmación de la política debe ser verdad en el código: revisa SDKs de terceros, analítica, informes de errores, mapas con telemetría.

### 4. Cuestionarios
Rellénalos con ella, desde `plan/PRIVACIDAD.md`, **con honestidad**: declarar de más no penaliza; declarar de menos es motivo de retirada.
- **Apple**: Privacidad de la app (datos recogidos, vinculados a la identidad, seguimiento), clasificación por edades, cifrado (`ITSAppUsesNonExemptEncryption: false` ya en `app.json`), derechos de contenido (si usa contenido de terceros con licencia).
- **Google**: Seguridad de los datos, clasificación de contenido (IARC), público objetivo, anuncios, acceso a la app (credenciales para revisores), app gubernamental / de noticias / sanitaria si aplica, permisos sensibles.
- Revisa la clasificación cada vez que añadas funciones sociales (chat, comentarios, contenido de usuarios).

### 5. Ficha y gráficos
- **Textos**: subtítulo (Apple, 30), palabras clave (Apple, 100, separadas por comas, sin repetir el nombre), descripción breve (Google, 80) y completa. Primero lo que resuelve, después cómo. **Nada que la app no haga hoy**; nada de «el mejor», «oficial», «n.º 1», «con IA» si no la hay. Si la app usa información de organismos públicos: aviso visible de «app independiente, no vinculada a <organismo>» y enlace a las fuentes oficiales.
- **Icono**: 1024×1024 sin transparencia para Apple; 512×512 para Google. Que se entienda en pequeño.
- **Capturas**: de la app real, en el tamaño que pida hoy cada consola. Trucos:
  - Mac: simulador del tamaño requerido + `xcrun simctl io booted screenshot captura.png`. Usa un build sin el menú de desarrollo.
  - Windows: capturas del móvil real; si el tamaño no coincide, Claude puede redimensionarlas/rellenar con el color de fondo con un script.
  - Las 3 primeras son las que se ven en búsquedas: que cuenten la propuesta de valor con un titular corto encima.
  - Google: gráfico destacado 1024×500.
- **Vídeo de vista previa** (opcional): Apple exige pista de audio aunque sea silencio y que se vea bien sin sonido.

### 6. Prueba cerrada de Google (cuenta personal nueva)
- Requisito: **al menos 12 testers apuntados durante 14 días seguidos** antes de poder solicitar producción (comprueba la cifra actual en la ayuda de Play Console).
- Recluta **15-20** (algunos se caen). Deben usar la cuenta de Google de su Play Store, aceptar desde el enlace de inscripción e **instalar desde Play**. Pídeles que no la desinstalen.
- Google no revisa la prueba cerrada hasta que la ficha y todo «Contenido de la app» estén completos: haz los puntos 3-5 antes.
- Subir builds nuevas durante la prueba no reinicia los 14 días: sigue arreglando.
- Hacia el día 10, pide a 2-3 testers su opinión por escrito: al solicitar producción, Google pregunta qué aprendiste y qué cambiaste.

### 7. Notas para el revisor (en inglés)
- Qué hace la app en 2 líneas; cómo probar la función principal paso a paso.
- **Cuenta demo** creada a propósito (sin verificación en dos pasos, con datos de ejemplo). La contraseña va en el campo de la consola, nunca en el repo.
- Lo que necesite algo especial (otro móvil, una ubicación, un archivo de ejemplo): el revisor puede estar en otro país.

## Salida

- `plan/CHECKLIST_TIENDAS.md` marcado, URLs de privacidad y soporte apuntadas en `ESTADO.md`.
- Textos de la ficha en `plan/FICHA.md` (por tienda, con recuento de caracteres) y respuestas de los cuestionarios: así la siguiente versión es copiar y ajustar.
- `plan/ESTADO.md`: fase → «publicación», HT marcado.
- Siguiente: **movil-publicar**.
