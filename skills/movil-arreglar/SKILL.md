---
name: movil-arreglar
description: Diagnostica y arregla errores de una app Expo/React Native con método - pantalla roja, la app se cierra, Metro no arranca, el móvil no conecta, fallos de build en EAS, "no funciona en Expo Go", estilos que no se aplican, teclado que tapa campos, login con Google que falla. Consulta primero las trampas conocidas y la documentación oficial. Úsala cuando digan "me da un error", "no funciona", "se cierra la app", "pantalla roja", "falla el build", o `/movil-arreglar`.
---

# movil-arreglar

> **Archivos compartidos**: `REGLAS.md`, `plantillas/` y `referencias/` están en la skill `movil`, carpeta hermana de esta (`../movil/`). Si no la encuentras, búscala (`**/movil/REGLAS.md`). Si en el proyecto no existe `plan/ESTADO.md`, propón empezar por la skill `movil`.

Lee `REGLAS.md` y `referencias/trampas.md` de la skill `movil`.

## Método

1. **Captura el error completo.** Pide una captura o el texto exacto (de la terminal de Metro, de la pantalla roja o del log de EAS en expo.dev). Si es un build de EAS, abre el log del paso que falla, no solo el resumen.
2. **Reprodúcelo**: ¿pasa siempre?, ¿en las dos plataformas?, ¿en Expo Go y en development build?, ¿solo en producción?
3. **Mira `referencias/trampas.md`**: muchos errores ya los hemos visto.
4. **Busca en la documentación oficial** y en los *issues* de GitHub de la librería implicada, con el mensaje de error exacto y la versión del SDK.
5. **Una hipótesis cada vez.** Cambia una sola cosa, prueba, y si no era eso, deshazla.
6. **Arreglado = comprobado en el móvil.** Después, commit `arregla: <qué>` y, si es una trampa nueva, añádela a `plan/ESTADO.md` → «Decisiones» para no repetirla.

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
- Si tras dos intentos no está claro, para y explícale la situación con opciones.
