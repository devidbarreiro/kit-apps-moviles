---
name: movil-actualizar
description: Mantiene y actualiza una app Expo ya publicada - decide entre EAS Update (cambio de código al momento, sin revisión) y una versión nueva en las tiendas (cambios nativos), sube versiones, planifica la v1.1 con lo que quedó fuera, actualiza el SDK de Expo y mide si la usan. Úsala cuando digan "arreglar un fallo de la app publicada", "nueva versión", "actualización", "EAS Update", "OTA", "subir el SDK de Expo", "v1.1", o `/movil-actualizar`.
---

# movil-actualizar

> **Archivos compartidos**: `REGLAS.md`, `plantillas/` y `referencias/` están en la skill `movil`, carpeta hermana de esta (`../movil/`). Si no la encuentras, búscala (`**/movil/REGLAS.md`). Si en el proyecto no existe `plan/ESTADO.md`, propón empezar por la skill `movil`.

Lee `REGLAS.md` y `referencias/trampas.md` de la skill `movil`, `plan/ESTADO.md` (versiones publicadas) y `plan/ALCANCE.md` («Se queda fuera»). Doc: https://docs.expo.dev/eas-update/introduction/.

## 1. ¿Actualización al momento o versión nueva?

| El cambio… | Vía |
|---|---|
| Solo toca código JS/TS, textos, estilos, imágenes del bundle | **EAS Update** (llega al abrir la app; sin revisión) |
| Añade o cambia una librería nativa, un permiso, un *config plugin*, el icono, el splash, `app.json` nativo | **Versión nueva** en las tiendas (build + revisión) |
| Solo datos (filas en la base de datos, contenido remoto) | Ni una ni otra: se cambia en el backend |
| Cambia mucho lo que la app hace o promete | Versión nueva (las tiendas no permiten cambiar la naturaleza de la app por EAS Update) |

### Configurar EAS Update (una vez, con build nuevo)
```bash
npx expo install expo-updates
eas update:configure
```
En `app.json`: `runtimeVersion: { "policy": "appVersion" }` (cada `version` es un «runtime»: una actualización solo llega a los binarios de esa versión). Cada perfil de `eas.json` con su `channel` (`preview`, `production`). Comprueba la doc actual antes de configurarlo.

### Publicar una actualización
```bash
eas update --channel preview --environment preview --message "Arregla el botón de guardar"
# probar en un móvil con el build preview; después:
eas update --channel production --environment production --message "Arregla el botón de guardar"
```
- **Siempre a `preview` primero** y probado en un móvil.
- Si sale mal: vuelve a publicar la actualización anterior (`eas update:republish`) o usa el *rollback* de EAS; compruébalo en la doc.
- Solo con su «sí» explícito para `production`.

## 2. Versión nueva

1. Sube `version` en `app.json` (semver: 1.0.0 → 1.0.1 arreglos, 1.1.0 funciones). El número de build lo sube EAS.
2. Si hay backend: **primero la base de datos, después la app.** Las migraciones deben seguir funcionando con la versión publicada (añadir columnas, no renombrar ni borrar lo que usa la versión vieja).
3. Build + submit + revisión como en `movil-publicar`. Novedades de la versión escritas para usuarios (Google: máx. 500 caracteres).
4. Al comparar «qué cambia», hazlo contra la versión **publicada**, no contra la última subida a TestFlight.
5. `ESTADO.md` → «Versiones publicadas» y `git tag v<versión>`.

## 3. La v1.1

- Recupera `ALCANCE.md` → «Se queda fuera» y lo que piden los usuarios. Repite `movil-alcance` → `movil-hitos` para la nueva versión (añade hitos H<n+1>… en la misma hoja de ruta).
- Una novedad grande también se puede anunciar con un **evento dentro de la app** de Apple (In-App Event): imágenes sin texto ni logos, máx. 31 días; se revisa aparte.

## 4. Actualizar el SDK de Expo

- Expo saca SDK nuevo varias veces al año; las tiendas acaban exigiendo versiones recientes de Xcode/Android. Planifica 1-2 actualizaciones de SDK al año, **una versión mayor cada vez**.
- Lee la guía de la versión (https://docs.expo.dev/workflow/upgrading-expo-sdk-walkthrough/ y el *changelog*), en una rama de git:
  ```bash
  npx expo install expo@^<nuevo> --fix
  npx expo-doctor
  ```
- Nuevo development build, prueba completa en móvil, y versión nueva en tiendas.
- Si el build local falla con errores de Swift/Xcode, compila en la nube de EAS (usa una versión de Xcode compatible) antes de parchear nada.

## 5. ¿La usan?

- En App Store Connect, «impresiones» no son descargas; mira «primeras descargas». Los datos llegan con 1-2 días de retraso.
- Lo que importa no es cuánta gente se registra, sino cuánta **vuelve** (día 1, día 7) y cuánta hace la acción principal. Si añades analítica o informes de errores (p. ej. Sentry), decláralo en privacidad **antes** de activarlo.
- Pedir permiso de notificaciones en el momento adecuado (tras una acción con sentido, no al abrir) multiplica cuántos aceptan.
- Mantén el backend despierto y los certificados/cuentas al día (Apple caduca cada año: si no renueva, la app desaparece de la tienda).
