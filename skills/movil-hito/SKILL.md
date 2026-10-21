---
name: movil-hito
description: Desarrolla el siguiente hito de la hoja de ruta de una app móvil Expo - lo planifica en pasos pequeños, consulta la documentación oficial de cada librería, programa, comprueba (TypeScript, expo-doctor, prueba en el móvil) y cierra con commit y plan/ESTADO.md al día. Úsala cuando digan "sigamos con la app", "siguiente hito", "programa H2", "añade la pantalla de…", "haz que la app…", o `/movil-hito`.
---

# movil-hito

> **Archivos compartidos**: `REGLAS.md`, `plantillas/` y `referencias/` están en la skill `movil`, carpeta hermana de esta (`../movil/`). Si no la encuentras ahí, búscala en `~/.claude/skills/movil/` o en `~/.claude/plugins/` (`**/movil/REGLAS.md`). Si en el proyecto no existe `plan/ESTADO.md`, propón empezar por la skill `movil`.

El bucle de construcción: **planear → consultar docs → programar → comprobar → ella prueba → guardar**. Un hito cada vez.

Lee `REGLAS.md`, `referencias/stack.md` y `referencias/trampas.md` de la skill `movil`; después `plan/ESTADO.md`, `plan/HOJA_DE_RUTA.md` y, si existe, `plan/hitos/H<n>.md`.

## 1. Planear

1. Identifica el hito actual en `ESTADO.md`. Si ya hay `plan/hitos/H<n>.md` a medias, retoma desde el primer paso sin marcar.
2. Si no existe, crea `plan/hitos/H<n>.md` con `plantillas/HITO.md`: objetivo, requisitos, criterios de «hecho» y **pasos de 15-30 minutos**, cada uno con algo visible en el móvil.
3. Si el diseño de una pantalla no está claro, pregunta antes: describe 2 opciones en palabras (o un esquema ASCII) y deja que elija.
4. Enséñale el plan en una lista corta y pide «adelante».

## 2. Consultar antes de programar

Para cada librería o API que vayas a usar:
- Busca primero en el SDK de Expo (https://docs.expo.dev/versions/latest/). Si hay módulo oficial, ese.
- Lee la doc de **la versión del SDK del proyecto** (mira `expo` en `package.json`). context7 si lo tienes; si no, WebFetch.
- Instala con `npx expo install`. Si trae código nativo o un *config plugin*, avisa: **no funcionará en Expo Go** y habrá que hacer un development build (`movil-probar`).
- Anota en `H<n>.md` la URL consultada y la librería añadida.

## 3. Programar

- Cambios pequeños, un paso cada vez. Tras cada paso, que ella lo vea en el móvil (recarga rápida automática; si no, sacude el móvil → Reload).
- Usa componentes de React Native y Expo antes que crear los tuyos: `Pressable`, `FlatList`, `expo-image` (no `Image` para fotos remotas), `SafeAreaView`/`useSafeAreaInsets`, `Stack`/`Tabs` de expo-router.
- Accesibilidad básica: `accessibilityLabel` en botones con solo icono, tamaño mínimo táctil ~44 pt, respeta el tamaño de letra del sistema.
- Textos para el usuario en su idioma, sin jerga técnica. Los errores que ve el usuario dicen qué hacer («Sin conexión. Inténtalo de nuevo»); el detalle técnico va al registro, no a la pantalla.
- Lógica en funciones puras con su test cuando haya cálculos o reglas. La primera vez que haga falta, configura Vitest (`npm install -D vitest` y un script `"test": "vitest run"` en `package.json`; comprueba la doc de Vitest).
- Si pide un permiso (cámara, ubicación, notificaciones, fotos): pídelo **en el momento en que se usa**, nunca al abrir la app; texto concreto del porqué en el *config plugin* del módulo; alternativa si dice que no. Desactiva en el plugin los permisos que no uses (ver trampas).
- Datos nuevos → actualiza `plan/PRIVACIDAD.md`.

## 4. Comprobar (antes de decir «hecho»)

```bash
npx tsc --noEmit
npx expo-doctor          # si cambiaron dependencias o app.json
npm test                 # si hay tests
```
Después, ella repasa **cada criterio de «hecho»** en su móvil (y en la otra plataforma si la tiene). Pide confirmación explícita de cada uno. Si algo falla → `movil-arreglar`.

Prueba también: sin conexión (modo avión), con texto muy largo, con lista vacía, girando el móvil si está permitido, y cerrando y abriendo la app.

## 5. Guardar

- Commit por paso que funcione; al cerrar el hito, `H<n>: <resumen>`.
- `H<n>.md`: marca pasos y verificación, fecha de fin, problemas encontrados.
- `HOJA_DE_RUTA.md`: marca el hito.
- `ESTADO.md`: progreso, último paso, siguiente paso exacto, decisiones. Si era el último hito del MVP, fase → «pruebas».
- Pregunta: ¿seguimos con el siguiente hito o lo dejamos aquí?

## Reglas duras

- No empieces un hito con el anterior roto.
- No añadas funcionalidades fuera de `ALCANCE.md`. Si las pide, apúntalas en «Se queda fuera» y pregunta si quiere cambiar el alcance (decisión registrada).
- Si llevas dos intentos sin arreglar un error, para, explica qué pasa y consulta la doc o el *issue tracker* de la librería antes de seguir probando cosas.
- Cuando todos los hitos del MVP estén hechos → **movil-probar**.
