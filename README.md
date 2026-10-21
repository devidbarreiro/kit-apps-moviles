# Kit de apps móviles para Claude Code

Un conjunto de *skills* para que **Claude Code te acompañe desde la idea hasta tener tu app publicada** en la App Store (iPhone) y en Google Play (Android), aunque no hayas programado nunca.

Tú pones la idea, decides y pruebas en tu móvil. Claude hace preguntas, planifica, programa, consulta la documentación oficial y te explica cada paso. Todo el plan queda apuntado en una carpeta `plan/` dentro de tu proyecto, así que puedes parar y seguir otro día sin perder el hilo.

Está hecho con lo que aprendimos publicando varias apps propias con **Expo** (la forma más sencilla y oficial de hacer apps de iPhone y Android con una sola base de código).

## Cómo funciona

Escribe **`/movil`** (o simplemente «quiero hacer una app») y Claude te guía por estas fases:

| Fase | Skill | Qué pasa |
|---|---|---|
| 0. Inicio | `movil` | Mira dónde estás y te lleva al siguiente paso. Úsala siempre para retomar. |
| 1. Entrevista | `movil-entrevista` | ¿Mac o Windows? ¿Qué móviles tienes? ¿Presupuesto? ¿Cuentas? Con eso sabe qué es posible. |
| 2. Idea | `movil-idea` | Para quién es, qué problema resuelve, qué tiene que poder hacer. Sale una lista de requisitos. |
| 3. Alcance | `movil-alcance` | Qué entra en la primera versión y qué se queda para después. |
| 4. Hitos | `movil-hitos` | El plan en etapas pequeñas; cada una termina con algo que ves en el móvil. |
| 5. Preparar | `movil-preparar` | Instala las herramientas, crea el proyecto y abre tu app en tu móvil. |
| 6. Backend | `movil-backend` | Solo si necesitas cuentas o datos en la nube (Supabase), seguro desde el primer día. |
| 7. Construir | `movil-hito` | Un hito cada vez: plan, documentación oficial, código, pruebas en tu móvil, guardado. |
| 8. Probar | `movil-probar` | La app instalada de verdad: development build, TestFlight, prueba con otras personas. |
| 9. Tiendas | `movil-tienda` | Cuentas, nombre, capturas, política de privacidad, cuestionarios, testers de Google. |
| 10. Publicar | `movil-publicar` | Envío a revisión, qué esperar y qué hacer si la rechazan. |
| 11. Actualizar | `movil-actualizar` | Arreglos al momento (EAS Update), versiones nuevas, la v1.1. |
| Cuando algo falla | `movil-arreglar` | Diagnóstico con método y las trampas que ya conocemos. |

## Qué necesitas

- Un ordenador **Mac o Windows** con unos 10 GB libres (30-40 GB más si en Mac quieres el simulador de iPhone, que es opcional).
- Tu móvil (iPhone, Android o los dos).
- **Claude Code** instalado (ver abajo) y una suscripción de Claude que lo incluya.
- Ganas de probar mucho en el móvil. No necesitas saber programar.

### ¿Mac o Windows?

- **Mac**: puedes hacerlo todo.
- **Windows**: Android completo. iPhone **también**, pero la app se compila en la nube con EAS (el servicio de Expo) y se prueba en un iPhone real; no hay simulador de iPhone. Para publicar en la App Store no necesitas Mac.

## Lo que cuesta publicar

| Qué | Precio | ¿Cuándo hace falta? |
|---|---|---|
| Apple Developer Program | 99 €/año | Para probar en iPhone con TestFlight y publicar en la App Store |
| Google Play Console | 25 $ (pago único) | Para publicar en Google Play |
| Expo / EAS | Gratis (con un número limitado de builds al mes) | Desde el principio |
| Backend (Supabase) | Gratis para empezar | Solo si la app tiene cuentas o datos en la nube |
| Web de privacidad y soporte | Gratis (GitHub Pages, Vercel…) | Antes de publicar |

Los precios pueden cambiar: Claude te dirá dónde comprobarlos. Desarrollar y probar en tu móvil con Expo Go es gratis; solo pagas cuando vas a publicar.

> **Aviso de Google Play**: si abres una cuenta personal nueva, Google exige una **prueba cerrada con al menos 12 personas durante 14 días seguidos** antes de dejarte publicar. Ve pensando en quiénes pueden ser.

## Instalación

### 1. Instala Claude Code

Sigue la guía oficial de instalación de Claude Code (busca «Claude Code install» en la documentación de Anthropic). Resumen:

- **Mac** (Terminal): `curl -fsSL https://claude.ai/install.sh | bash`
- **Windows** (PowerShell): `irm https://claude.ai/install.ps1 | iex`

Abre una terminal nueva y escribe `claude` para comprobar que arranca.

### 2. Instala el kit

**Opción A, como plugin (recomendada).** Descarga o clona esta carpeta en tu ordenador y, dentro de Claude Code, escribe:

```
/plugin marketplace add /ruta/a/kit-apps-moviles
/plugin install kit-movil@kit-movil
```

(En Windows, la ruta tipo `C:\Users\tu-usuario\Downloads\kit-apps-moviles`.) Si el kit está publicado en GitHub, en lugar de la ruta puedes poner `usuario/repositorio`.

Con el plugin, las skills se llaman con el prefijo del plugin: `/kit-movil:movil`, `/kit-movil:movil-hito`… También funcionan sin escribir el comando: basta con decir «quiero hacer una app» o «sigamos con la app».

**Opción B, copiando las skills.** Copia el contenido de `skills/` en tu carpeta personal de skills de Claude Code:

- **Mac**: `mkdir -p ~/.claude/skills && cp -R skills/* ~/.claude/skills/`
- **Windows** (PowerShell): `New-Item -ItemType Directory -Force "$env:USERPROFILE\.claude\skills"; Copy-Item -Recurse skills\* "$env:USERPROFILE\.claude\skills\"`

Así se llaman sin prefijo: `/movil`, `/movil-hito`… Copia **todas** las carpetas: comparten las reglas, plantillas y referencias de la skill `movil`.

### 3. Empieza

1. Crea una carpeta para tus apps (por ejemplo `apps` en tu carpeta de usuario).
2. Abre una terminal en esa carpeta y escribe `claude`.
3. Escribe `/movil` (o `/kit-movil:movil`) y responde a las preguntas.

Para **seguir otro día**: abre Claude Code en la carpeta de tu app y escribe `/movil`. Leerá `plan/ESTADO.md` y te dirá dónde lo dejaste y qué toca.

## Consejos

- **Una cosa cada vez.** Termina un hito y pruébalo en el móvil antes de pedir lo siguiente.
- **Si algo no te cuadra, dilo.** «Esto no es lo que quería» es la frase más útil que le puedes decir a Claude.
- **No compartas claves ni contraseñas en el chat** si no hace falta, y nunca las subas a GitHub. Claude te dirá dónde va cada cosa.
- **Claude no paga, no abre cuentas por ti ni publica sin que se lo digas.** Esas decisiones son tuyas.
- Si tienes el servidor MCP **context7** instalado en Claude Code, Claude lo usará para consultar la documentación actualizada de cada librería. Si no, usará la web oficial.

## Qué hay en el kit

```
.claude-plugin/        manifiesto del plugin
skills/
  movil/               punto de entrada + REGLAS.md (las normas para Claude)
    plantillas/        perfil, requisitos, alcance, hoja de ruta, estado, hito, privacidad,
                       checklist de tiendas, política de privacidad
    referencias/       stack recomendado, trampas técnicas, tiendas y rechazos reales
  movil-*/             una carpeta por fase
docs/lecciones.md      lo que aprendimos, en lenguaje llano
```

## Aviso

Las plantillas legales (política de privacidad) son orientativas y no sustituyen el consejo de un profesional. Las reglas de las tiendas y los precios cambian: el kit le pide a Claude que los compruebe en las fuentes oficiales, pero la última palabra la tienen Apple y Google.
