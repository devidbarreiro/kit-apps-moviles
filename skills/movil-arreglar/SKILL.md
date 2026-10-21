---
name: movil-arreglar
description: Diagnostica y arregla errores de una app Expo/React Native con método - pantalla roja, la app se cierra, Metro no arranca, el móvil no conecta, fallos de build en EAS, "no funciona en Expo Go", estilos que no se aplican, teclado que tapa campos, login con Google que falla. Consulta primero las trampas conocidas y la documentación oficial. Úsala cuando digan "me da un error", "no funciona", "se cierra la app", "pantalla roja", "falla el build", o `/movil-arreglar`.
---

# movil-arreglar

> **Archivos compartidos**: `REGLAS.md`, `plantillas/` y `referencias/` están en la skill `movil`, carpeta hermana de esta (`../movil/`). Si no la encuentras ahí, búscala en `~/.claude/skills/movil/` o en `~/.claude/plugins/` (`**/movil/REGLAS.md`). Si en el proyecto no existe `plan/ESTADO.md`, propón empezar por la skill `movil`.

Lee `REGLAS.md` y `referencias/trampas.md` de la skill `movil`.

## Primero, tranquilidad (díselo con estas palabras o parecidas)

> Los errores son normales; casi todos tienen arreglo y nada de lo que hemos guardado se pierde. Vamos a mirarlo con calma.

**Qué necesitas de ella:**
1. **El error entero, copiado y pegado** en el chat (no un resumen ni «me sale un error»). En la terminal: selecciona desde la primera línea roja hasta el final y copia (Mac: ⌘C; Windows Terminal: Ctrl+Mayús+C). En el móvil (pantalla roja): una captura. En un build de EAS: el enlace del build en expo.dev.
2. **Qué estaba haciendo** justo antes y si ha pasado más veces.
3. Que **borre del texto** contraseñas, claves o correos antes de pegarlo.

**Cuándo parar y pedir ayuda a una persona** (díselo tal cual si ocurre): si tras dos intentos el error sigue igual; si el arreglo pide pagar, borrar datos de usuarios, tocar la cuenta de Apple o Google, o cambiar algo fuera de la carpeta del proyecto; o si ella ya no entiende lo que está pasando. Puede abrir un *issue* en el repositorio del kit con la plantilla «Algo no funciona».

**Volver atrás sin romper nada** (si el cambio que lo rompió aún no está guardado): `git stash` aparta los cambios y deja la app como en el último punto de guardado (se recuperan con `git stash pop`). Si ya estaba guardado: `git log --oneline` para ver los puntos y `git revert <id>` para deshacer uno. Nada de `git reset --hard` sin su «sí» (ver `REGLAS.md` §6).

**Retomar otro día**: antes de cerrar, apunta en `plan/ESTADO.md` → «Bloqueos y esperas» el error, lo probado y la siguiente idea. Al volver, `/movil` lo leerá.

## Método

1. **Captura el error completo.** Pide una captura o el texto exacto (de la terminal de Metro, de la pantalla roja o del log de EAS en expo.dev). Si es un build de EAS, abre el log del paso que falla, no solo el resumen.
2. **Reprodúcelo**: ¿pasa siempre?, ¿en las dos plataformas?, ¿en Expo Go y en development build?, ¿solo en producción?
3. **Mira `referencias/trampas.md`**: muchos errores ya los hemos visto.
4. **Busca en la documentación oficial** y en los *issues* de GitHub de la librería implicada, con el mensaje de error exacto y la versión del SDK.
5. **Una hipótesis cada vez.** Cambia una sola cosa, prueba, y si no era eso, deshazla.
6. **Arreglado = comprobado en el móvil**, con evidencia (la salida limpia del comando, el build en verde, o lo que ella confirma ver en el móvil). Después, commit `arregla: <qué>` y, si es una trampa nueva, añádela a `plan/ESTADO.md` → «Decisiones» para no repetirla.
7. **Si era una trampa que el kit no conocía**, propónle abrir un *issue* en el repositorio del kit con la plantilla «Trampa nueva» (síntoma, error exacto sin datos personales, arreglo y versión del SDK). Redáctaselo tú para que solo tenga que pegarlo. Así llega a la siguiente persona.

## Primeros auxilios (en este orden)

```bash
npx expo-doctor                 # dependencias incompatibles con el SDK
npx expo install --check        # versiones fuera del SDK → npx expo install --fix
npx tsc --noEmit                # errores de tipos
npx expo start --clear          # limpia la caché de Metro
```
Si nada: borra `node_modules` y reinstala (`rm -rf node_modules && npm install`; en Windows PowerShell `Remove-Item -Recurse -Force node_modules; npm install`).

## Síntomas frecuentes

| Síntoma | Causa probable | Ver |
|---|---|---|
| «… is not available in Expo Go» / módulo nativo `null` | Librería nativa en Expo Go | development build (`movil-probar`) |
| Funciona en desarrollo, en producción sale vacía o rota | Variables de entorno que no están en EAS; módulo que se carga tarde | trampas → Configuración |
| El build de EAS falla en «Install dependencies» | `package-lock.json` desincronizado o versiones fuera del SDK | `npx expo install --fix`, reinstala, commit del lockfile |
| El build falla en «Run fastlane» / compilación iOS | Credenciales, capacidades, Xcode | trampas → Builds |
| Estilos que no se aplican en un botón | NativeWind + `style` como función | trampas → NativeWind |
| El teclado tapa el campo | `KeyboardAvoidingView` mal configurado | trampas → Teclado |
| Google Sign-In: `DEVELOPER_ERROR` | Falta un cliente OAuth Android con la SHA-1 correcta | trampas → Login con Google |
| El móvil no conecta con Metro | Wifi distinta, cortafuegos, VPN | `npx expo start --tunnel` |
| Pantalla en blanco al abrir | Error en `_layout` raíz o splash que no se oculta | mira el log de Metro; `SplashScreen.hideAsync()` |

## Reglas duras

- Nunca «arregles» desactivando la comprobación (TypeScript `any`, `// @ts-ignore`, quitar una regla de RLS, borrar un test) sin decirlo y sin su permiso.
- No toques `ios/` ni `android/` a mano: se regeneran con `npx expo prebuild`. Los cambios nativos van en `app.json` o en un *config plugin*.
- No parchees librerías de Expo (patch-package) salvo como último recurso, documentado y con fecha para quitarlo.
- Si tras dos intentos no está claro, para y explícale la situación con opciones (ver «Cuándo parar»).
- Nunca inventes comandos ni opciones para «probar a ver»: compruébalos con `--help` o en la doc.
