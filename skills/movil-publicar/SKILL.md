---
name: movil-publicar
description: Envía una app Expo a revisión y la publica en App Store y Google Play con EAS Build y EAS Submit - comprobaciones previas, envío, qué esperar de la revisión, cómo responder a un rechazo de Apple o Google (con ejemplos reales generalizados) y publicación manual. Úsala cuando digan "publicar la app", "enviar a revisión", "subir a la App Store", "subir a Google Play", "me han rechazado la app", "rejection", "Resolution Center", o `/movil-publicar`.
---

# movil-publicar

> **Archivos compartidos**: `REGLAS.md`, `plantillas/` y `referencias/` están en la skill `movil`, carpeta hermana de esta (`../movil/`). Si no la encuentras ahí, búscala en `~/.claude/skills/movil/` o en `~/.claude/plugins/` (`**/movil/REGLAS.md`). Si en el proyecto no existe `plan/ESTADO.md`, propón empezar por la skill `movil`.

Lee `REGLAS.md` y `referencias/tiendas.md` de la skill `movil`, `plan/CHECKLIST_TIENDAS.md` y `plan/ESTADO.md`. **No envíes nada a revisión ni publiques sin su «sí» explícito.**

## 1. Antes de enviar

- [ ] `plan/CHECKLIST_TIENDAS.md` completo (si falta algo, vuelve a `movil-tienda`).
- [ ] `npx tsc --noEmit`, `npx expo-doctor` y `npx jest` (si hay tests): limpios.
- [ ] `version` en `app.json` correcta (1.0.0 la primera). El número de build lo sube EAS.
- [ ] Probado el build de **producción** en un móvil real de cada plataforma.
- [ ] Backend despierto y en producción; cuenta demo funciona.
- [ ] Ningún permiso de más: `npx expo config --type introspect` y revisa los `NS…UsageDescription` (iOS) y `permissions` (Android). Cada texto, concreto y con el nombre de la app.
- [ ] Congela funcionalidades: mientras dure la revisión, solo arreglos.

## 2. Enviar

**iOS**
```bash
eas build --platform ios --profile production
eas submit --platform ios --latest
```
En App Store Connect: selecciona el build en la versión, revisa ficha, privacidad y notas para el revisor, elige **publicación manual** («Publicar manualmente esta versión») y pulsa «Enviar a revisión». La publicación manual da margen para comprobar entre la aprobación y la salida.

**Android**
```bash
eas build --platform android --profile production
eas submit --platform android --latest
```
(El primer AAB, a mano en Play Console.) Cuenta personal nueva: primero la prueba cerrada de 14 días y luego «Solicitar acceso a producción». Después, crea la versión en **Producción**; considera un despliegue por fases (p. ej. 20 %) en las siguientes versiones.

## 3. Qué esperar

- Apple: normalmente 1-2 días; la primera puede tardar más. Google: de horas a una semana; la primera revisión y la solicitud de producción, varios días.
- Mientras espera: prepara la web, el anuncio, y la lista de los primeros usuarios. Apunta en `ESTADO.md` → «Bloqueos y esperas» la fecha de envío.

## 4. Si la rechazan

Tranquilidad: es normal, sobre todo la primera vez. Proceso:

1. **Lee el mensaje entero** y apunta la directriz citada (p. ej. «Guideline 5.1.1»). Busca su texto exacto en las App Review Guidelines o en la política de Google.
2. **Encuentra la causa real** en la app, la ficha o las capturas. A menudo no es el código: es una captura vieja, un texto ambiguo o el backend caído.
3. **Arregla primero**, luego responde. Si el arreglo es solo de ficha/metadatos, no hace falta build nuevo; si toca la app, sí (sube versión de build y reenvía).
4. **Responde en inglés, corto y verificable**: qué has cambiado, dónde verlo (pantalla, pasos), y nada más. No discutas, no expliques lo que la app «era antes» (puede reactivar el problema). Si no entiendes qué pantalla lo provocó, pregúntalo.
5. Si no estás de acuerdo: en Apple, responde en el Centro de resoluciones, pide una llamada («Meet with Apple») o recurre al App Review Board; en Google, usa el formulario de apelación del correo.
6. Si Apple avisa de **revisión ampliada** («extended review»), no reenvíes sin cambios: responde antes.
7. Guarda cada rechazo y tu respuesta en `plan/RECHAZOS.md`.

Rechazos que hemos visto y cómo se resolvieron: `referencias/tiendas.md` → «Rechazos reales».

## 5. Aprobada

- Publica (si elegiste manual) cuando ella lo diga.
- Comprueba que aparece en la tienda de su país y en otro (si no sale en la UE, revisa el estado de comerciante DSA).
- Instálala desde la tienda en un móvil limpio y prueba lo básico.
- `ESTADO.md`: tabla «Versiones publicadas», fase → «mantenimiento». Etiqueta en git: `git tag v1.0.0`.
- Siguiente: **movil-actualizar**.
