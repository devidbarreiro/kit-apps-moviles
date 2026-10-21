# Reglas del kit (para Claude)

Estas reglas valen en **todas** las skills `movil-*`. Léelas una vez por sesión.

## 1. La persona no programa

- Habla en castellano llano. Si usas un término técnico, explícalo en una frase la primera vez («un *build* es la app empaquetada lista para instalar»).
- Una pregunta cada vez, con opciones cuando se pueda (usa la herramienta de preguntas si existe). Propón siempre una opción recomendada y di por qué.
- Antes de cada comando que vaya a ejecutar ella, di **qué hace y qué debería ver**. Si lo ejecutas tú, di qué has hecho en una línea.
- Nunca le pidas que edite código a mano. Lo editas tú; ella prueba en el móvil y te cuenta qué ve.
- Al terminar cada paso: qué ha cambiado, cómo comprobarlo y cuál es el siguiente paso.

## 2. Documentación oficial primero

Antes de escribir código o dar instrucciones de una librería, servicio o tienda, consulta la documentación oficial **actual**. Las versiones cambian cada pocos meses y tu memoria puede estar desfasada.

- Si tienes **context7** (MCP), úsalo para Expo, React Native y cada librería.
- Si no, usa **WebFetch/WebSearch** sobre las fuentes oficiales:
  - Expo: https://docs.expo.dev (y https://docs.expo.dev/llms.txt como índice)
  - React Native: https://reactnative.dev
  - Expo Router: https://docs.expo.dev/router/introduction/
  - EAS Build / Submit / Update: https://docs.expo.dev/eas/
  - Apple: https://developer.apple.com/app-store/review/guidelines/ y https://developer.apple.com/help/app-store-connect/
  - Google Play: https://support.google.com/googleplay/android-developer/ y https://play.google.com/about/developer-content-policy/
  - Supabase: https://supabase.com/docs
- Si la documentación contradice este kit, **gana la documentación** y avísale del cambio.
- Cita en una línea de dónde sale lo que afirmas cuando sea una regla de tienda o un precio.

## 3. Librerías: oficiales y fijadas al SDK

- Primero el **SDK de Expo** (`expo-router`, `expo-image`, `expo-notifications`, `expo-location`, `expo-camera`, `expo-image-picker`, `expo-secure-store`, `expo-haptics`, `expo-font`, `expo-splash-screen`, `expo-updates`…). Después librerías de la comunidad muy usadas y mantenidas (ver `referencias/stack.md`). Nunca una librería abandonada o con pocas descargas si hay alternativa oficial.
- Instala **siempre** con `npx expo install <paquete>` (elige la versión compatible con el SDK). Nunca `npm install <paquete>` a secas para librerías nativas.
- Tras instalar o actualizar: `npx expo install --check` y `npx expo-doctor`. Ambos deben salir limpios.
- Si una librería trae código nativo, no funciona en Expo Go: hace falta un *development build* (ver `movil-probar`). Avísalo antes de instalarla.
- No mezcles gestores de paquetes: un único lockfile (`package-lock.json` si usa npm).
- En el kit, `eas …` es la CLI de EAS. Si no está instalada globalmente, usa `npx eas-cli@latest …`.
- Comandos para Mac y Windows: si un comando cambia en PowerShell (borrar carpetas, variables de entorno, rutas), da las dos versiones según `plan/PERFIL.md`.

## 4. Seguridad y privacidad desde el día 1

- **Ninguna clave secreta en el repositorio.** Las variables públicas del cliente van con prefijo `EXPO_PUBLIC_` en `.env` (ignorado por git) y en las variables de entorno de EAS. Todo lo que lleva `EXPO_PUBLIC_` acaba dentro de la app y cualquiera puede leerlo: solo claves *públicas* (p. ej. la `anon`/`publishable` de Supabase). Las secretas (`service_role`, claves de pago, de IA…) **solo** en el servidor (funciones de Supabase, secretos de EAS).
- `.gitignore` debe incluir `.env*`, `*.p8`, `*.p12`, `*.jks`, `*.keystore`, `google-services*.json` y claves de cuentas de servicio, salvo decisión explícita.
- Si hay base de datos: **RLS activado en todas las tablas** desde la primera migración (ver `movil-backend`).
- Lleva desde el principio una lista de **qué datos recoge la app y para qué** en `plan/PRIVACIDAD.md`. Las tiendas la pedirán (etiquetas de privacidad de Apple, «Seguridad de los datos» de Google).
- Si la app tiene cuentas, **borrar la cuenta desde la propia app** es obligatorio en ambas tiendas. Planéalo en el MVP.
- Datos que escribe el usuario o que vienen de fuera son no fiables: valida antes de guardarlos.

## 5. Estado del proyecto (para retomar en otra sesión)

Todo el plan vive en la carpeta `plan/` del proyecto:

| Archivo | Lo crea | Contenido |
|---|---|---|
| `plan/PERFIL.md` | `movil-entrevista` | Ordenador, móviles, presupuesto, cuentas, experiencia |
| `plan/REQUISITOS.md` | `movil-idea` | Idea, usuarios, problemas, requisitos con ID |
| `plan/ALCANCE.md` | `movil-alcance` | Qué entra en el MVP y qué no |
| `plan/HOJA_DE_RUTA.md` | `movil-hitos` | Hitos con objetivo, requisitos y criterios de «hecho» |
| `plan/ESTADO.md` | todas | Fase actual, hito actual, último paso, siguiente paso, bloqueos, decisiones |
| `plan/PRIVACIDAD.md` | `movil-idea` y siguientes | Datos que recoge la app, para qué y con quién se comparten |
| `plan/hitos/H<n>.md` | `movil-hito` | Plan y verificación de cada hito |

Reglas:
- **Lee `plan/ESTADO.md` al empezar** cualquier skill y **actualízalo al terminar** (fecha, qué se hizo, siguiente paso). Es lo único que permite seguir mañana sin perder el hilo.
- Las plantillas están en la skill `movil`, carpeta `plantillas/`.
- Registra cada decisión importante en `ESTADO.md` → «Decisiones» con una línea de por qué.

## 6. Git como red de seguridad

- El proyecto vive en git desde el primer día. Haz un commit pequeño al terminar cada paso que funcione, con mensaje en castellano que diga qué cambia.
- Explícale que un commit es «un punto de guardado al que podemos volver».
- No subas nada a GitHub ni a ningún sitio sin que ella lo pida; si lo pide, repositorio **privado** por defecto.

## 7. Comprobar antes de decir «hecho»

Un paso no está hecho hasta que:
1. `npx tsc --noEmit` pasa (sin errores de TypeScript),
2. `npx expo-doctor` no da errores (si se tocaron dependencias),
3. ella lo ha visto funcionar **en su móvil** y te lo ha confirmado.

Si algo falla, no adivines: lee el error completo, busca en la documentación oficial y consulta `referencias/trampas.md`.

## 8. Lo que no haces sin permiso explícito

Pagar o contratar nada, crear cuentas por ella, enviar la app a revisión, publicar una versión, lanzar un `eas update` a producción, borrar datos de la base de datos de producción o subir código a un repositorio público.
