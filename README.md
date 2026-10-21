# Kit de apps móviles para Claude Code

> **v0.1 · en pruebas.** Funciona, pero todavía lo está usando poca gente. Si algo falla, [cuéntanoslo](https://github.com/devidbarreiro/kit-apps-moviles/issues/new/choose).

Un conjunto de *skills* para que **Claude Code te acompañe desde la idea hasta tener tu app publicada** en la App Store (iPhone) y en Google Play (Android), aunque no hayas programado nunca.

Tú pones la idea, decides y pruebas en tu móvil. Claude hace preguntas, planifica, programa, consulta la documentación oficial y te explica cada paso. Todo el plan queda apuntado en una carpeta `plan/` dentro de tu proyecto, así que puedes parar y seguir otro día sin perder el hilo.

Está hecho con lo que aprendimos publicando varias apps propias con **Expo**, la forma más sencilla y oficial de hacer apps de iPhone y Android con una sola base de código.

Web: https://devidbarreiro.github.io/kit-apps-moviles/

## Un ejemplo de principio a fin

Ana es fisioterapeuta y quiere una app para que sus pacientes apunten los ejercicios que hacen en casa.

1. Instala Claude Code y el kit (media hora, una sola vez).
2. Escribe `/kit-movil:movil`. Claude crea el proyecto en `~/apps/ejercicios-casa` y le hace una entrevista de 10 minutos: tiene Windows y un iPhone, y quiere salir primero en iOS.
3. Le cuenta la idea. Salen 9 requisitos; Claude le propone dejar 5 para la primera versión y **sin cuentas ni servidor**: los datos se guardan en el móvil.
4. Claude parte el trabajo en 6 hitos. Esa misma tarde, Ana ve una app en blanco con su nombre en su iPhone (H0).
5. Paga los 99 USD de Apple y, con la app aún vacía, la sube a TestFlight para comprobar que cuentas y firmas funcionan (H0.5).
6. Durante 5 semanas, un hito por semana: Claude programa, Ana prueba en el móvil y dice qué no le cuadra.
7. Tres pacientes la usan una semana (HP). Salen dos fallos; se arreglan.
8. Prepara la ficha, las capturas y la política de privacidad con Claude (HT) y la envía a revisión (HL).
9. Apple la aprueba en dos días. Ana la publica cuando quiere.
10. Cada arreglo pequeño posterior llega a los móviles sin pasar otra vez por revisión (EAS Update).

**Tiempo realista**: con 5-8 horas a la semana, **entre 6 y 12 semanas** para una primera versión pequeña. Súmale los plazos que no dependen de ti: la verificación de las cuentas de Apple y Google (de días a un par de semanas), la revisión de Apple (la mitad de las apps en menos de 24 h y el 90 % en menos de 48 h) y la de Google (hasta 7 días o más en cuentas nuevas). Si abres una cuenta **personal** nueva de Google Play, añade **14 días** de prueba cerrada con 12 personas. Es una estimación, no una promesa.

## Cómo funciona

Escribe **`/kit-movil:movil`** (o simplemente «quiero hacer una app») y Claude te guía por estas fases:

| Fase | Skill | Qué pasa |
|---|---|---|
| 0. Inicio | `movil` | Crea el proyecto o mira dónde estás y te lleva al siguiente paso. Úsala siempre para retomar. |
| 1. Entrevista | `movil-entrevista` | ¿Mac o Windows? ¿Qué móviles tienes? ¿Presupuesto? ¿Cuentas? Con eso sabe qué es posible y te ayuda a decidir cuándo pagar Apple. |
| 2. Idea | `movil-idea` | Para quién es, qué problema resuelve, qué tiene que poder hacer. Sale una lista de requisitos. |
| 3. Alcance | `movil-alcance` | Qué entra en la primera versión y qué se queda para después. |
| 4. Hitos | `movil-hitos` | El plan en etapas pequeñas; cada una termina con algo que ves en el móvil. |
| 5. Preparar (H0) | `movil-preparar` | Instala las herramientas, configura el proyecto y abre tu app en tu móvil. |
| 6. App en blanco en la tienda (H0.5) | `movil-probar` | La app vacía, en TestFlight y en la prueba interna de Google, antes de programar nada. |
| 7. Construir | `movil-hito` | Un hito cada vez: plan, documentación oficial, código, pruebas en tu móvil, guardado. |
| Si hace falta | `movil-backend` | Solo si hay cuentas, sincronización entre móviles o datos compartidos (Supabase). Por defecto, no. |
| 8. Probar (HP) | `movil-probar` | La app instalada de verdad y usada por otras personas una semana. |
| 9. Tiendas (HT) | `movil-tienda` | Nombre, capturas, política de privacidad, cuestionarios, testers de Google. |
| 10. Publicar (HL) | `movil-publicar` | Envío a revisión, qué esperar y qué hacer si la rechazan. |
| 11. Actualizar | `movil-actualizar` | Arreglos al momento (EAS Update), versiones nuevas, la v1.1. |
| Cuando algo falla | `movil-arreglar` | Diagnóstico con método y las trampas que ya conocemos. |

## Qué necesitas

- Un ordenador **Mac o Windows** con unos 10 GB libres (30-40 GB más si en Mac quieres el simulador de iPhone, que es opcional).
- Tu móvil (iPhone, Android o los dos).
- **Claude Code** y un plan de pago de Claude (Pro o superior; el plan gratuito no incluye Claude Code).
- Ganas de probar mucho en el móvil. **No necesitas saber programar.**

### ¿Mac o Windows?

- **Mac**: puedes hacerlo todo.
- **Windows**: Android completo. iPhone **también**: la app se compila en la nube con EAS (el servicio de Expo) y se prueba en un iPhone real; no hay simulador de iPhone. Para publicar en la App Store no necesitas Mac.
- **Windows + iPhone sin pagar Apple**: solo puedes probar con Expo Go, que **solo abre el SDK de Expo más reciente**. Si sale uno nuevo a mitad del proyecto, toca actualizar el proyecto o pagar Apple para usar tu propia versión de prueba. La entrevista te ayuda a decidir.

## Lo que cuesta

Precios **verificados en 2026-10** en las webs oficiales. Cambian: Claude los vuelve a comprobar cuando los necesitas ([fuentes](skills/movil/referencias/tiendas.md#requisitos-con-fecha-verificado-2026-10)).

| Qué | Precio | ¿Cuándo hace falta? |
|---|---|---|
| Claude Pro | 20 USD/mes (17 USD/mes pagando el año) | Desde el principio |
| Apple Developer Program | 99 USD/año (en tu moneda al darte de alta) | Para TestFlight, para probar tu propia versión en iPhone y para publicar en la App Store |
| Google Play Console | 25 USD, pago único | Para publicar en Google Play |
| Expo / EAS | Gratis: 15 builds de Android y 15 de iOS al mes, en cola lenta | Desde el principio |
| Servidor (Supabase) | Gratis para empezar | Solo si la app tiene cuentas o datos compartidos |
| Web de privacidad y soporte | Gratis (GitHub Pages, Vercel…) | Antes de publicar |

Desarrollar y probar en tu móvil con Expo Go es gratis.

> **Aviso de Google Play**: si abres una cuenta **personal** nueva, Google exige una **prueba cerrada con al menos 12 personas durante 14 días seguidos** antes de dejarte publicar. Busca unas 15 con móvil Android (alguna se cae). Si no las tienes, sal primero en iPhone.

## Instalación

Para cada paso: **si ves X, va bien; si ves Y, haz Z.**

### 1. Abre una terminal

Una terminal es una ventana donde se escriben órdenes.

- **Mac**: ⌘ + espacio, escribe «Terminal» y pulsa Intro.
- **Windows**: menú Inicio → escribe «PowerShell» → ábrelo. Verás algo como `PS C:\Users\ana>`. (Si no empieza por `PS`, estás en otra consola: cierra y abre PowerShell.)

### 2. Instala git

En **Windows**, en PowerShell:

```powershell
winget install Git.Git
```

- Si ves `Successfully installed`: va bien. **Cierra y abre** PowerShell.
- Si ves que `winget` no se reconoce: descárgalo de https://git-scm.com/downloads/win y deja todas las opciones por defecto.

Claude Code ya no exige git en Windows (si no lo encuentra, usa PowerShell), pero el kit guarda el historial de tu app con git y lo necesita para instalarse desde GitHub.

En **Mac**, escribe `git --version`. Si ves un número de versión, ya lo tienes. Si te ofrece instalar las «herramientas de línea de comandos», acepta y espera a que termine.

### 3. Instala Claude Code

- **Mac**: `curl -fsSL https://claude.ai/install.sh | bash`
- **Windows** (PowerShell): `irm https://claude.ai/install.ps1 | iex`

El instalador no enseña progreso mientras descarga: espera. Después, **abre una terminal nueva** y escribe:

```
claude --version
```

- Si ves un número de versión seguido de `(Claude Code)`: va bien.
- Si ves `command not found` o «no se reconoce»: cierra todas las terminales y abre una nueva. Si sigue, mira la [guía oficial de instalación](https://code.claude.com/docs/en/setup).

Escribe `claude` para arrancarlo. La primera vez abre el navegador para que inicies sesión con tu cuenta de Claude.

### 4. Instala el kit

Dentro de Claude Code (donde escribes los mensajes), escribe estas dos órdenes, una cada vez:

```
/plugin marketplace add devidbarreiro/kit-apps-moviles
```

- Si ves `Successfully added marketplace: kit-movil`: va bien.
- Si ves un error de red o de git: comprueba tu conexión y que git está instalado (paso 2), y repite.

```
/plugin install kit-movil@kit-movil
```

Qué significa: «instala el plugin `kit-movil` del catálogo `kit-movil`». Lo que va antes de la `@` es el plugin; lo que va después, el catálogo (*marketplace*) de donde sale. Aquí se llaman igual.

- Se abre un panel con lo que trae el kit. Elige **Install for you (user scope)**: así lo tienes en todos tus proyectos.
- Si al final ves `Plugin is now active.`: listo.
- Si ves `Run /reload-plugins to apply.`: Claude Code lo recarga solo; si no, escribe `/reload-plugins`.
- Para comprobarlo, escribe `/kit-movil:` y deben aparecer las skills (`/kit-movil:movil`, `/kit-movil:movil-hito`…).

### 5. Empieza

1. Cierra Claude Code (`/exit`).
2. En la terminal, quédate en tu carpeta de usuario (es donde se abre la terminal) y escribe `claude`.
3. Escribe `/kit-movil:movil` (o «quiero hacer una app») y responde a las preguntas. Claude creará el proyecto en `~/apps/<nombre>` (Mac) o `C:\apps\<nombre>` (Windows).

Para **seguir otro día**: abre una terminal, entra en la carpeta de tu app (`cd ~/apps/<nombre>` o `cd C:\apps\<nombre>`), escribe `claude` y después `/kit-movil:movil`. Claude leerá `plan/ESTADO.md` y te dirá dónde lo dejaste.

### Actualizar el kit

Los catálogos que no son de Anthropic no se actualizan solos. Para traer la última versión, dentro de Claude Code:

```
/plugin marketplace update kit-movil
```

Después, `/plugin` → pestaña **Installed** → `kit-movil` → **Update now**. (Desde la terminal, fuera de Claude Code: `claude plugin update kit-movil@kit-movil`.) Si prefieres que se actualice solo: `/plugin` → pestaña **Marketplaces** → `kit-movil` → **Enable auto-update**. Lo que cambia en cada versión está en [CHANGELOG.md](CHANGELOG.md).

### Otra forma: copiar las skills a mano

Si no puedes usar plugins: en GitHub, **Code → Download ZIP**, descomprímelo, abre una terminal dentro de la carpeta descomprimida y copia el contenido de `skills/` en tu carpeta personal de skills:

- **Mac**: `mkdir -p ~/.claude/skills && cp -R skills/* ~/.claude/skills/`
- **Windows** (PowerShell): `New-Item -ItemType Directory -Force "$env:USERPROFILE\.claude\skills"; Copy-Item -Recurse skills\* "$env:USERPROFILE\.claude\skills\"`

Copia **todas** las carpetas: comparten las reglas, plantillas y referencias de la skill `movil`. Así se llaman sin prefijo (`/movil`, `/movil-hito`…), y para actualizar hay que repetir la copia.

## Los permisos de Claude Code

Claude Code te pide permiso antes de ejecutar órdenes o cambiar archivos. Verás un cuadro con la orden y opciones para aceptar o rechazar. No es un error: es para que tú decidas.

**Aprueba con tranquilidad** (es lo normal en este kit):
- Leer archivos de tu proyecto.
- Crear o editar archivos **dentro de la carpeta de tu app**.
- Órdenes que empiezan por `npm`, `npx`, `expo` o `eas build`, y `git add` / `git commit`, dentro de tu proyecto.

**Para y pregunta antes de aprobar** si la orden:
- **Borra** algo (`rm`, `Remove-Item`, `del`, `git reset --hard`, `git clean`).
- **Paga**, **publica** o **envía** (`eas submit`, `eas update --channel production`, `git push`).
- Toca **fuera de la carpeta de tu app** (`sudo`, instalar programas, cambiar ajustes del sistema).
- Pide **contraseñas** o claves.

Pregúntale a Claude «¿qué hace exactamente esto y por qué?». El kit le pide que te avise antes de cualquiera de estas. Si dudas, rechaza y pregunta: no se rompe nada por decir que no.

### Límites de uso del plan

El plan Pro tiene un límite de uso que **se reinicia cada 5 horas** y otro **semanal**, compartidos entre el chat de Claude y Claude Code. Si ves un aviso de **límite alcanzado**:

1. No pasa nada: tu proyecto y tu plan están guardados.
2. Espera a la hora de reinicio que indica el aviso.
3. Abre Claude Code en la carpeta de tu app y escribe `/kit-movil:movil`: seguirá desde `plan/ESTADO.md`.

Para gastar menos: una tarea cada vez y `/clear` al empezar un hito nuevo (el hilo está en `plan/`). Si te quedas corto a menudo, el plan Max da más uso.

## Si algo falla

1. **Copia el error entero y pégalo** en el chat (borra antes contraseñas, claves o correos). En la terminal, selecciona desde la primera línea roja hasta el final; en el móvil, haz una captura.
2. Escribe `/kit-movil:movil-arreglar` o «me da este error».
3. Claude debe enseñarte **pruebas** de que está arreglado (la salida del comando o qué mirar en el móvil), no solo decir «listo».
4. **Volver atrás** sin romper nada: pide a Claude «vuelve al último punto de guardado» (usará `git stash` o `git revert`, que no borran historia). Si te propone `git reset --hard`, pregunta qué se pierde.
5. **Para y pide ayuda a una persona** si tras dos intentos sigue igual, si el arreglo pide pagar, borrar datos o tocar tus cuentas de Apple o Google, o si ya no entiendes qué pasa. Puedes [abrir un *issue*](https://github.com/devidbarreiro/kit-apps-moviles/issues/new/choose).
6. Si lo resolvisteis y era algo nuevo, [compártelo como «Trampa nueva»](https://github.com/devidbarreiro/kit-apps-moviles/issues/new/choose): ayudará a la siguiente persona.

## Decisiones que no sabes tomar: lo que recomendamos

| Decisión | Por defecto | Por qué |
|---|---|---|
| **Identificador de la app** (*bundle id* en iPhone, *package* en Android) | `com.tunombreapellido.nombreapp`, en minúsculas y sin guiones ni tildes. Ej.: Ana García con «Diario de plantas» → `com.anagarcia.diarioplantas`. Si tienes dominio (`midominio.com`) → `com.midominio.diarioplantas` | No se puede cambiar una vez creada la app en las tiendas. Que sea tuyo y no dependa del nombre comercial. |
| **Cuenta como persona o empresa** | **Persona** | Más rápido; empresa pide número D-U-N-S y que exista legalmente. Como persona, tu nombre sale como vendedor. |
| **Comerciante en la UE (DSA)** | App gratis y sin actividad comercial → normalmente «no comerciante». Si cobras o es parte de tu negocio → «comerciante» (tu dirección y teléfono se ven en la ficha) | Ambas tiendas lo preguntan. Esto no es asesoramiento legal: confírmalo en su ayuda. |
| **¿Servidor (backend)?** | **No**: los datos se guardan en el móvil | Solo si hay cuentas, sincronización entre móviles o datos compartidos entre personas. |
| **¿Qué tienda primero?** | La de tu móvil. Con cuenta personal nueva de Google y sin ~15 testers con Android: **iPhone primero** | Evitas bloquearte en la prueba cerrada de Google. |
| **¿Cuándo pagar Apple?** | Antes de H0.5 si vas a iPhone y tu MVP tiene algo nativo (notificaciones push, login con Google/Apple, mapas nativos…); si no, al terminar el MVP | Lo nativo necesita tu propia versión de prueba, que en iPhone exige la cuenta de pago. |

## Palabras que vas a oír

| Palabra | Qué significa |
|---|---|
| **Terminal** | Una ventana donde se escriben órdenes. Mac: «Terminal». Windows: «PowerShell». |
| **Ruta** | La dirección de una carpeta, p. ej. `/Users/ana/apps/miapp` o `C:\apps\miapp`. |
| **Node** | El programa que hace funcionar las herramientas de Expo en tu ordenador. Se instala una vez. |
| **npm / npx** | npm instala piezas de código (paquetes); `npx` ejecuta una herramienta sin instalarla para siempre. |
| **Skill** | Un manual de instrucciones que Claude lee para hacer una tarea concreta. Este kit son 13. |
| **Plugin / marketplace** | Un plugin es un paquete de skills; un *marketplace* es el catálogo de donde se instala. |
| **Expo** | Las herramientas con las que se hace la app para iPhone y Android a la vez. |
| **SDK de Expo** | La versión del conjunto de piezas de Expo que usa tu proyecto (p. ej. «SDK 57»). Sale una nueva varias veces al año. |
| **Expo Go** | Una app gratuita de las tiendas que abre tu proyecto escaneando un **QR** (el cuadrado de puntos que sale en la terminal). Solo abre el SDK más reciente. |
| **Development build** | Tu propia versión de Expo Go, con las piezas nativas de tu app dentro. En iPhone necesita la cuenta de Apple de pago. |
| **Build** | La app empaquetada, lista para instalar. |
| **EAS** | El servicio en la nube de Expo que empaqueta la app, la firma y la sube a las tiendas. |
| **APK / AAB** | Formatos de app de Android. El APK se instala directamente; el AAB es el que se sube a Google Play. |
| **Bundle id / package** | El identificador único de tu app en las tiendas (p. ej. `com.anagarcia.diarioplantas`). |
| **Keystore** | La «llave» con la que se firma la app de Android. EAS la crea; guarda una copia. |
| **SHA-1** | La huella de esa llave. Solo hace falta si usas login con Google. |
| **TestFlight** | La app de Apple para instalar versiones de prueba antes de publicarlas. |
| **Cuenta de servicio** (*service account*) | Una «llave» de Google para que EAS pueda subir la app a Google Play por ti. |
| **Backend** | El servidor donde se guardan datos compartidos (cuentas, fotos…). Muchas apps no lo necesitan. |
| **RLS** | En Supabase, las reglas que deciden quién puede leer o escribir cada fila de datos. Obligatorias en el kit. |
| **TypeScript** | El lenguaje en el que Claude escribe tu app. Avisa de muchos errores antes de abrirla. |
| **Hito** | Una etapa pequeña del plan que termina con algo que ves en el móvil. |
| **Git / commit** | El historial del proyecto; un commit es un punto de guardado al que se puede volver. |
| **«Sacude el móvil → Reload»** | Si la app no se actualiza sola en Expo Go, agita el móvil: sale un menú; pulsa «Reload» para recargarla. |

## Consejos

- **Una cosa cada vez.** Termina un hito y pruébalo en el móvil antes de pedir lo siguiente.
- **Si algo no te cuadra, dilo.** «Esto no es lo que quería» es la frase más útil que le puedes decir a Claude.
- **No compartas claves ni contraseñas en el chat** si no hace falta, y nunca las subas a GitHub. Claude te dirá dónde va cada cosa.
- **Claude no paga, no abre cuentas por ti ni publica sin que se lo digas.** Esas decisiones son tuyas.
- **Ten dos terminales abiertas**: en una hablas con Claude; en la otra, cuando Claude te lo pida, arrancas la app (`npx expo start`) o inicias sesión en servicios.
- Claude consulta la documentación oficial en internet antes de programar.

## Qué hay en el kit

```
.claude-plugin/        manifiestos del plugin y del catálogo
skills/
  movil/               punto de entrada + REGLAS.md (las normas para Claude)
    plantillas/        perfil, requisitos, alcance, hoja de ruta, estado, hito, privacidad,
                       checklist de tiendas, política de privacidad
    referencias/       stack recomendado, trampas técnicas, tiendas (con requisitos fechados)
  movil-*/             una carpeta por fase
docs/                  la web del kit (GitHub Pages) y lecciones.md, lo aprendido en lenguaje llano
CHANGELOG.md           qué cambia en cada versión
ROADMAP.md             lo que viene después
```

## Colaborar

- ¿Algo no funciona? [Abre un *issue* «Algo no funciona»](https://github.com/devidbarreiro/kit-apps-moviles/issues/new/choose).
- ¿Has resuelto un problema que el kit no conocía? [«Trampa nueva»](https://github.com/devidbarreiro/kit-apps-moviles/issues/new/choose).
- Lo que viene: [ROADMAP.md](ROADMAP.md).

## Aviso y licencia

Este kit **no es asesoramiento legal, fiscal ni profesional**. Las plantillas legales (política de privacidad) son orientativas. Las reglas de las tiendas, los precios y los requisitos cambian: el kit le pide a Claude que los compruebe en las fuentes oficiales, pero la última palabra la tienen Apple, Google y, en lo legal, un profesional. Lo usas bajo tu responsabilidad.

Licencia [MIT](LICENSE) © 2026 David Barreiro. Puedes usarlo, copiarlo y modificarlo libremente, manteniendo el aviso de licencia.
