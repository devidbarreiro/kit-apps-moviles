---
name: movil-preparar
description: Hito H0 de una app móvil - instala las herramientas (Node, git, Expo, EAS; Xcode/Android Studio solo si conviene) en Mac o Windows, crea el proyecto con create-expo-app (expo-router + TypeScript), configura app.json, eas.json, .gitignore y .env, y abre la app en el móvil con Expo Go. Úsala cuando digan "prepara el proyecto", "instala lo necesario", "crea el proyecto Expo", "empezar a programar la app", "H0", o `/movil-preparar`.
---

# movil-preparar (H0)

> **Archivos compartidos**: `REGLAS.md`, `plantillas/` y `referencias/` están en la skill `movil`, carpeta hermana de esta (`../movil/`). Si no la encuentras ahí, búscala en `~/.claude/skills/movil/` o en `~/.claude/plugins/` (`**/movil/REGLAS.md`). Si en el proyecto no existe `plan/ESTADO.md`, propón empezar por la skill `movil`.

Objetivo: al terminar, la persona ve **su** app (en blanco, con su nombre) en **su** móvil, y el proyecto está en git.

Lee `REGLAS.md` y `referencias/stack.md` de la skill `movil`, y `plan/PERFIL.md`. Antes de dar comandos, comprueba en https://docs.expo.dev/get-started/create-a-project/ y https://docs.expo.dev/get-started/set-up-your-environment/ si algo ha cambiado.

## 1. Herramientas

Comprueba qué hay antes de instalar (`node -v`, `git --version`, `npx --version`). Necesitas **Node LTS** (la que indique la doc de Expo) y **git**.

**Mac**
- Si falta, propón instalar Homebrew (https://brew.sh) y luego `brew install node git`. O el instalador oficial de https://nodejs.org (LTS).
- Watchman es opcional (`brew install watchman`).
- **Xcode (App Store, 30-40 GB)** solo si quiere simulador de iPhone o builds locales y tiene espacio. No es necesario para empezar.
- **Android Studio** solo si quiere emulador de Android.

**Windows**
- Instalador oficial de Node LTS (https://nodejs.org) y Git for Windows (https://git-scm.com/downloads/win). O en PowerShell: `winget install OpenJS.NodeJS.LTS` y `winget install Git.Git`. Git hace falta para el proyecto (historial) aunque Claude Code ya no lo exija para funcionar. Después de instalar, **cierra y abre** la terminal para que encuentre `node` y `git`.
- Recomienda **Windows Terminal + PowerShell**. Rutas sin espacios ni tildes (p. ej. `C:\apps\miapp`; evita la carpeta de usuario si su nombre tiene espacios o tildes): evitan errores raros.
- Android Studio solo si quiere emulador. **iOS: no hay simulador ni Xcode en Windows**; se compila en la nube con EAS (ver `movil-probar`).

**Ambos**
- En el móvil: instalar **Expo Go** desde App Store / Google Play.
- Cuenta en https://expo.dev (gratis) y `npx eas-cli@latest login` (lo escribe ella en su terminal: pide usuario y contraseña) (o `npm install -g eas-cli` si prefiere tenerlo instalado; explícale la diferencia).

## 2. El proyecto

Lo normal es que el proyecto **ya exista**: la skill `movil` lo crea al principio con `create-expo-app` y mete `plan/` y git dentro. Compruébalo: en la carpeta actual debe haber `package.json` (con `expo` en dependencias), `app.json` y `plan/`.

**Si no existe** (alguien empezó sin la skill `movil`), créalo en este orden, **nunca** dentro de una carpeta que ya tenga cosas:

Mac (Terminal):
```bash
mkdir -p ~/apps
cd ~/apps
npx create-expo-app@latest <nombre-app> --yes
cd <nombre-app>
mkdir plan
git status        # si dice «not a git repository»: git init
```
Windows (PowerShell):
```powershell
New-Item -ItemType Directory -Force C:\apps
cd C:\apps
npx create-expo-app@latest <nombre-app> --yes
cd <nombre-app>
New-Item -ItemType Directory plan
git status        # si dice «not a git repository»: git init
```
`<nombre-app>` en minúsculas, sin espacios ni tildes (de ahí salen `name` y `slug`). Si ya había una carpeta `plan/` suelta de antes, **cópiala** dentro del proyecto nuevo (no muevas archivos ocultos ni fusiones carpetas a mano).

**SDK**: si `plan/PERFIL.md` fija un SDK distinto del que trae el proyecto (p. ej. porque la Expo Go de la tienda ya abre otro), crea el proyecto con ese SDK (`--template default@sdk-<NN>`, compruébalo en https://docs.expo.dev/more/create-expo/) o actualízalo con `npx expo install expo@^<NN>.0.0 --fix`. Anota el SDK final en `PERFIL.md`.

Después:
```bash
npx expo install --check
npx expo-doctor
```

Si la plantilla trae pantallas de ejemplo, ofrece `npm run reset-project` (si existe en `package.json`) para dejarla limpia. Comprueba después que `plan/` sigue ahí.

## 3. Configuración mínima

- **`app.json`** (o `app.config.ts` si necesita variables): `name` (el que verá el usuario), `slug`, `scheme` (para enlaces profundos), `version: "1.0.0"`, `orientation: "portrait"`, `ios.supportsTablet: false` (salvo que el MVP incluya iPad), `ios.infoPlist.ITSAppUsesNonExemptEncryption: false`.
- **Identificadores**: `ios.bundleIdentifier` y `android.package` con formato `com.<dominio-o-nombre>.<app>` (minúsculas, sin guiones ni tildes). **No se pueden cambiar una vez creada la app en las consolas**: decídelos con ella ahora. Si no sabe qué poner, propón por defecto `com.<nombreapellido>.<app>` (p. ej. Ana García con «Diario de plantas» → `com.anagarcia.diarioplantas`); si tiene dominio propio, al revés (`midominio.com` → `com.midominio.diarioplantas`). Usa el mismo en iOS y Android.
- **No pongas** `ios.buildNumber` ni `android.versionCode`: los gestiona EAS (siguiente punto).
- **`eas.json`**: `npx eas-cli@latest init` (interactivo: que lo ejecute ella) y `npx eas-cli@latest build:configure -p all`. Ajusta (comprueba claves en https://docs.expo.dev/build/eas-json/):
  - `cli.appVersionSource: "remote"`,
  - perfiles `development` (`developmentClient: true`, `distribution: "internal"`), `preview` (`distribution: "internal"`) y `production` (`autoIncrement: true`),
  - cada perfil con su `environment` (`development`, `preview`, `production`).
- **`.gitignore`**: añade lo de la regla 4 de `REGLAS.md` (`.env*`, claves `.p8/.p12/.jks/.keystore`, `google-services*.json`, `/ios` y `/android` si no se van a tocar a mano).
- **`.env`** (no se sube) con las variables `EXPO_PUBLIC_…` que hagan falta y un **`.env.example`** (sí se sube: añade `!.env.example` al `.gitignore`) con los nombres y valores vacíos.
- **Variables de entorno en EAS**: los builds en la nube **no ven tu `.env`**. Cada variable se crea también en EAS (`eas env:set --name … --value … --environment production --visibility plaintext`; en versiones antiguas de la CLI era `eas env:create`: comprueba con `eas env --help`). Las `EXPO_PUBLIC_` como `plaintext` (acaban dentro de la app igualmente).
- **Configuración que falta = error claro.** Crea un módulo (p. ej. `src/config.ts`) que lea las variables y, si falta alguna en producción, lance un error con el nombre de la variable. Sin esto, un build sin variables sale «vacío» sin avisar.

## 4. Estructura

Propón esta y explícala en 3 líneas:
```
app/            pantallas y navegación (expo-router); archivos finos que importan de src/
src/components/ piezas reutilizables de interfaz
src/features/   cada funcionalidad: pantallas, lógica y datos juntos
src/lib/        utilidades, cliente del backend, config
src/theme.ts    colores, tamaños, tipografía
assets/         icono, splash, imágenes, fuentes
plan/           el plan (no es código)
```
Lógica (cálculos, reglas, validación) en funciones puras de `src/` sin React: se pueden probar sin móvil.

## 5. Primer arranque

Pídele que lo arranque **ella en una segunda terminal** (se queda corriendo y muestra un QR):
```bash
npx expo start
```
Escanea el QR con la cámara (iPhone) o con Expo Go (Android). Mismo wifi en ordenador y móvil; si la red no lo permite, `npx expo start --tunnel`.

Cambia el texto de la pantalla inicial por el nombre de la app y comprueba que se actualiza solo en el móvil. Ese es el «hecho» de H0.

Qué debe ver ella: en la terminal, un QR y la línea `Metro waiting on exp://…`; en el móvil, Expo Go abre la app con el nombre que acabas de poner. Si Expo Go dice que el proyecto usa un SDK incompatible, ve a «Problemas típicos».

## 6. Cierre

- `npx tsc --noEmit` y `npx expo-doctor` limpios.
- Primer commit: `H0: proyecto Expo creado`.
- Si quiere copia en la nube: repositorio **privado** en GitHub (solo si lo pide).
- `plan/ESTADO.md`: H0 ✅, siguiente → **H0.5 · app en blanco en la tienda** con **movil-probar** (sección H0.5). Si ella decide no pagar todavía Apple ni Google, salta a H1 con **movil-hito** y deja H0.5 anotado como pendiente en «Bloqueos y esperas».

## Problemas típicos

- `EACCES` / permisos al instalar global → usa `npx` en vez de instalar global; nunca `sudo npm`.
- El móvil no conecta → misma wifi, cortafuegos de Windows (permitir Node), o `--tunnel`.
- Expo Go dice que el SDK no es compatible → la Expo Go de la tienda solo abre el SDK más reciente. Opciones: actualizar el proyecto a ese SDK (`npx expo install expo@^<NN>.0.0 --fix`, una versión mayor cada vez) o pasar a *development build* (en iPhone necesita Apple Developer de pago). Ver la decisión de Expo Go en `plan/PERFIL.md`.
- Más en `referencias/trampas.md` de la skill `movil`.
