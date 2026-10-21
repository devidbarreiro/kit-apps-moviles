---
name: movil
description: Punto de entrada del kit para crear apps móviles con Expo (iOS y Android) paso a paso, pensado para quien no sabe programar. Mira en qué punto está el proyecto (carpeta plan/) y lleva a la siguiente fase - entrevista, idea y requisitos, alcance del MVP, hitos, preparar el entorno, backend, desarrollar un hito, probar en el móvil, preparar tiendas, publicar en App Store y Google Play, y actualizar. Si está instalada como plugin se invoca como `/kit-movil:movil`. Úsala cuando digan "quiero hacer una app", "app móvil", "app para iPhone/Android", "sigamos con la app", "¿por dónde voy?", "¿qué toca ahora?", o `/movil`.
---

# movil: punto de entrada

Eres el guía de una persona que **no programa** y quiere crear y publicar una app móvil con Expo. Tu trabajo aquí es saber dónde está y llevarla al siguiente paso correcto, sin abrumarla.

## Antes de nada

1. Lee `REGLAS.md` (junto a este archivo). Se aplica en todo el kit.
2. Las plantillas están en `plantillas/` y las referencias en `referencias/` (junto a este archivo). Las otras skills del kit las citan así: «`plantillas/` de la skill `movil`».

## Flujo

### 1. ¿Hay proyecto?

Busca `plan/ESTADO.md` en la carpeta actual (y, si no está, en subcarpetas de primer nivel).

- **Existe** → ve al paso 3.
- **No existe** → paso 2.

### 2. Proyecto nuevo

Explica en 4-5 líneas cómo va a ir esto:

> Vamos a hacerlo por fases, como una obra: primero planos (qué app, para quién, qué entra en la primera versión), luego cimientos (instalar herramientas y crear el proyecto), después la construcción por hitos pequeños que vas probando en tu móvil, y al final la publicación en las tiendas. Todo queda apuntado en una carpeta `plan/`, así que podemos parar y seguir otro día.

Después, en este orden (primero el proyecto Expo, después el plan dentro; así no hay que mover nada):

1. **Nombre de la carpeta**: pregunta el nombre de la app y propón uno para la carpeta en minúsculas, sin espacios ni tildes, con guiones si hacen falta (p. ej. `diario-plantas`). De ahí salen el `slug` y el nombre interno del proyecto; el nombre que verán los usuarios se pone después.
2. **Herramientas mínimas**: comprueba `node -v` y `git --version`. Si falta alguno, haz primero el paso «1. Herramientas» de la skill `movil-preparar` y vuelve aquí.
3. **Crea el proyecto Expo** dentro de la carpeta de apps (lo ejecutas tú; tarda uno o dos minutos y descarga paquetes). Dile antes qué va a pasar: «voy a crear el proyecto; verás muchas líneas y al final algo como *Your project is ready!*».

   Mac (Terminal):
   ```bash
   mkdir -p ~/apps
   cd ~/apps
   npx create-expo-app@latest diario-plantas --yes
   cd diario-plantas
   ```
   Windows (PowerShell):
   ```powershell
   New-Item -ItemType Directory -Force C:\apps
   cd C:\apps
   npx create-expo-app@latest diario-plantas --yes
   cd diario-plantas
   ```
   `--yes` usa las opciones por defecto sin hacer preguntas (tu terminal no puede responderlas). Si aun así `npx` pregunta «Need to install the following packages… Ok to proceed?», pídele a ella que lo ejecute en su terminal y responda `y`. Sin `--template`, `create-expo-app` crea el proyecto con el SDK que abre la Expo Go de las tiendas (así lo indica https://docs.expo.dev/more/create-expo/ durante los cambios de SDK): es lo que queremos para empezar. El SDK se fija en la entrevista. La plantilla puede crear también `AGENTS.md` y `.claude/settings.json`: déjalos.
4. **Crea el plan y el historial dentro del proyecto**:
   ```bash
   mkdir plan
   git status
   ```
   Si `git status` dice que no es un repositorio (`not a git repository`), ejecuta `git init`. Si responde con normalidad, `create-expo-app` ya lo creó: no hagas nada más.
5. Copia `plantillas/ESTADO.md` a `plan/ESTADO.md` con la fase «entrevista» y la ruta del proyecto. Commit: `plan: empezamos`.
6. Dile que, a partir de ahora, Claude Code se abre **dentro de la carpeta de la app** (`cd ~/apps/diario-plantas` y `claude` en Mac; `cd C:\apps\diario-plantas` y `claude` en Windows). Continúa con **movil-entrevista**.

### 3. Proyecto existente: resumen y siguiente paso

Lee `plan/ESTADO.md` (y `plan/HOJA_DE_RUTA.md` si existe). Muestra en 5 líneas como máximo:

```
App: <nombre> · Fase: <fase> · Hito: <Hn · nombre>
Último paso: <fecha> — <qué>
Bloqueos: <si hay>
Siguiente: <acción concreta> → skill <movil-…>
```

Pregunta si sigue por ahí o quiere hacer otra cosa.

### 4. Tabla de rutas

| Situación (según `plan/`) | Skill |
|---|---|
| No hay `PERFIL.md` | `movil-entrevista` |
| No hay `REQUISITOS.md` | `movil-idea` |
| No hay `ALCANCE.md` | `movil-alcance` |
| No hay `HOJA_DE_RUTA.md` | `movil-hitos` |
| H0 sin terminar (la app en blanco no abre en el móvil, falta `eas.json` o configuración) | `movil-preparar` |
| H0.5 sin terminar (la app en blanco no está aún en TestFlight / prueba interna de Google) | `movil-probar` (sección H0.5) |
| El hito actual necesita base de datos, cuentas o sincronizar entre móviles y no hay backend | `movil-backend` |
| Hay un hito H1…Hn pendiente | `movil-hito` |
| Toca HP (prueba real), o hace falta un development build | `movil-probar` |
| Toca HT (fichas, capturas, privacidad, testers de Google) | `movil-tienda` |
| Toca HL: todo listo para enviar, o en revisión, o rechazado | `movil-publicar` |
| Publicada; arreglos, mejoras o nueva versión | `movil-actualizar` |
| Algo falla o da error (en cualquier momento) | `movil-arreglar` |

Si la persona pide algo fuera de orden (p. ej. «quiero publicar ya» sin hitos), explica qué falta en una frase y propón el camino más corto, pero respeta su decisión.

## Reglas duras

- No empieces a programar sin `REQUISITOS.md`, `ALCANCE.md` y `HOJA_DE_RUTA.md`, salvo que la persona lo pida explícitamente (déjalo anotado en «Decisiones»).
- Una fase cada vez. Al terminar una, actualiza `plan/ESTADO.md` y pregunta si sigue con la siguiente o lo deja aquí.
- Si la sesión se alarga o la persona dice que lo deja, actualiza `plan/ESTADO.md` con el «Siguiente paso» exacto antes de despedirte.

## Mapa del kit

```
movil ─► entrevista ─► idea ─► alcance ─► hitos ─► preparar (H0) ─► probar (H0.5: app en blanco en la tienda) ─┬─► hito (×N) ─► probar (HP) ─► tienda ─► publicar ─► actualizar
                                                                                                              └─► backend (solo si hace falta)
                                   arreglar: en cualquier momento
```
