---
name: movil-probar
description: Lleva una app Expo del ordenador al móvil de verdad - el hito H0.5 (la app en blanco en TestFlight y en la prueba interna de Google antes de programar), Expo Go frente a development build, builds en la nube con EAS (también desde Windows para iPhone), instalar en iPhone (TestFlight o dispositivo registrado) y en Android (APK o prueba interna de Google Play), y una prueba real con otras personas. Úsala cuando digan "quiero probarla en mi móvil", "no funciona en Expo Go", "development build", "TestFlight", "pásasela a un amigo", "hacer un build", "H0.5", "app en blanco en la tienda", o `/movil-probar`.
---

# movil-probar

> **Archivos compartidos**: `REGLAS.md`, `plantillas/` y `referencias/` están en la skill `movil`, carpeta hermana de esta (`../movil/`). Si no la encuentras ahí, búscala en `~/.claude/skills/movil/` o en `~/.claude/plugins/` (`**/movil/REGLAS.md`). Si en el proyecto no existe `plan/ESTADO.md`, propón empezar por la skill `movil`.

Lee `REGLAS.md` y `referencias/trampas.md` de la skill `movil`, `plan/PERFIL.md` y `plan/ESTADO.md`. Doc: https://docs.expo.dev/develop/development-builds/introduction/ y https://docs.expo.dev/build/introduction/.

## Las tres formas de tener la app en el móvil

Explícaselas así:

| Forma | Qué es | Cuándo | Coste |
|---|---|---|---|
| **Expo Go** | Una app «contenedor» de Expo que abre tu proyecto escaneando un QR | Al principio, mientras solo uses librerías incluidas en Expo Go | Gratis |
| **Development build** | Tu propia versión de «Expo Go» con tus librerías nativas dentro; sigue cargando el código desde tu ordenador | En cuanto añadas algo nativo (mapas nativos, login con Google, push, widgets, compras…) | Android gratis; iPhone requiere Apple Developer de pago |
| **Build de producción** | La app final, como la bajaría un usuario | Prueba real, TestFlight, tiendas | Igual |

Regla: cuando una librería diga que necesita *config plugin* o «no disponible en Expo Go», toca development build. Mejor pronto que tarde: es donde salen los problemas nativos.

## H0.5 · App en blanco en la tienda

Se hace justo después de H0, **antes de programar funciones**. Objetivo: que la app vacía llegue a TestFlight y a la prueba interna de Google. Así salen pronto los problemas de cuentas, certificados, firma y nombres, cuando no hay nada que romper. Requisitos: H0 hecho, cuentas de Apple y/o Google **pagadas y verificadas** (la verificación tarda días: si aún no está, anótalo en «Bloqueos y esperas» y sigue con H1).

1. **Comprobaciones**: `app.json` con nombre, `slug`, bundle id / package **definitivos**, `version: "1.0.0"`; `eas.json` con el perfil `production`; `npx expo-doctor` limpio.
2. **iPhone** (lo ejecuta ella en su segunda terminal; es interactivo la primera vez):
   ```bash
   eas build --platform ios --profile production
   eas submit --platform ios --latest
   ```
   EAS le pedirá iniciar sesión con su Apple ID y crear certificados: que acepte que EAS los gestione. Si la app no existe aún en App Store Connect, `eas submit` ofrece crearla (compruébalo en https://docs.expo.dev/submit/ios/). Cuando Apple termine de procesarla (de minutos a una hora), en App Store Connect → TestFlight añade su Apple ID como **probador interno** e instálala con la app TestFlight.
3. **Android**:
   ```bash
   eas build --platform android --profile production
   ```
   Da un archivo **AAB**. Ella crea la app en Play Console y, en **Pruebas → Prueba interna**, crea una versión y **sube ese primer AAB a mano** (descárgalo desde el enlace del build en expo.dev). Se añade como tester con su cuenta de Google, abre el enlace de inscripción en el móvil e instala desde Play. Deja que EAS genere la *keystore* (la llave que firma la app) y **guarda una copia** en un gestor de contraseñas.
4. **Hecho cuando** ella abre la app en blanco instalada desde TestFlight y/o desde Google Play. Enseña la evidencia: el build «finished» en expo.dev, el estado en TestFlight o en la prueba interna, y lo que ve en el móvil.
5. Si algo falla, `movil-arreglar`. Apunta en `plan/ESTADO.md` el bundle id, el `ascAppId` si EAS lo muestra, y que H0.5 está hecho. Commit: `H0.5: app en blanco en las tiendas`.

## Development build

```bash
npx expo install expo-dev-client
eas build --profile development --platform android     # APK instalable directamente
eas build --profile development --platform ios         # iPhone físico (registro previo)
npx expo start --dev-client
```

- **iPhone físico**: antes, `eas device:create` (interactivo: lo ejecuta ella) y abrir el enlace en el iPhone para registrarlo; activar «Modo desarrollador» en Ajustes → Privacidad y seguridad. Necesita Apple Developer de pago.
- **Simulador de iPhone** (solo Mac con Xcode): perfil con `ios.simulator: true`.
- **Windows + iPhone**: todo lo anterior funciona desde Windows; EAS compila en la nube. La alternativa sin registrar el móvil es TestFlight (más abajo).
- Cada vez que añadas o cambies algo **nativo** (librería, permiso, plugin, icono), hay que hacer **otro** development build. Cambios solo de código JS: no.

## Cupo de builds

El plan gratuito de EAS tiene un número limitado de builds en la nube al mes (compartido por todos los proyectos de la cuenta; se reinicia el día 1). Un build fallido también gasta cupo y número de build. Para no malgastarlo:
- Antes de cada build: `npx tsc --noEmit`, `npx expo-doctor`, y `npx expo config --type public` para revisar la configuración final.
- En Mac con Xcode y espacio: `eas build --local` compila en tu ordenador sin gastar cupo (necesita fastlane y bastante disco).

## Build de prueba para otras personas

Si H0.5 está hecho, las cuentas, los certificados y la app en las consolas ya existen: esto es repetir los mismos comandos con la versión nueva.

**iPhone → TestFlight** (necesita Apple Developer y la app creada en App Store Connect):
```bash
eas build --platform ios --profile production
eas submit --platform ios --latest
```
- La primera vez EAS pide iniciar sesión con Apple (con verificación en dos pasos) y crea certificados y perfiles: deja que los gestione EAS. Hazlo en una terminal interactiva (si la app usa Iniciar sesión con Apple o push, EAS solo activa esas capacidades en modo interactivo).
- Tras «uploaded», Apple tarda unos minutos más en procesarlo. `eas submit` a veces se queda sin dar señales y aun así entrega: mira TestFlight antes de repetir.
- **Prueba interna** (hasta 100 personas del equipo de App Store Connect): sin revisión. **Prueba externa** (hasta 10.000 por enlace público): la primera build pasa una revisión rápida.

**Android**:
- Rápido: build `preview` (APK) y comparte el enlace de descarga de EAS.
- Por Google Play (recomendado antes de publicar): build `production` (AAB). **El primer AAB se sube a mano** en Play Console → Pruebas → Prueba interna. Después, `eas submit --platform android` con una cuenta de servicio de Google Play (guía: https://docs.expo.dev/submit/android/).
- Deja que EAS genere la *keystore* y **descarga una copia** a un gestor de contraseñas.

## La prueba real (hito HP)

1. 2-5 personas que no sean ella, con sus móviles, una semana.
2. Dales 3 tareas concretas («crea una nota y búscala») y pregunta qué les costó; no expliques cómo se usa antes.
3. Apunta lo que falle en `plan/hitos/HP.md` y arréglalo con `movil-hito` / `movil-arreglar`.
4. Prueba en el build de **producción**, no solo en desarrollo: hay errores que solo salen ahí (variables que faltan, módulos que se cargan tarde).
5. Si va a Google Play con cuenta personal nueva, aprovecha para reclutar a los **12+ testers** de la prueba cerrada (ver `movil-tienda`).

## Salida

- Tras **H0.5**: `plan/ESTADO.md` con H0.5 ✅ y siguiente → H1 con **movil-hito**.
- Tras **HP**: `plan/ESTADO.md`: fase → «tiendas»; qué builds hay, en qué móviles se ha probado, problemas abiertos.
  Siguiente: **movil-tienda**.
