---
name: movil-alcance
description: Recorta los requisitos de una app móvil a un MVP publicable - qué entra en la primera versión, qué se queda fuera y qué obliga la tienda aunque no sea "la idea" (privacidad, borrar cuenta, soporte). Deja plan/ALCANCE.md. Úsala cuando digan "qué hago primero", "MVP", "primera versión", "es demasiado", "alcance", o `/movil-alcance`.
---

# movil-alcance

> **Archivos compartidos**: `REGLAS.md`, `plantillas/` y `referencias/` están en la skill `movil`, carpeta hermana de esta (`../movil/`). Si no la encuentras ahí, búscala en `~/.claude/skills/movil/` o en `~/.claude/plugins/` (`**/movil/REGLAS.md`). Si en el proyecto no existe `plan/ESTADO.md`, propón empezar por la skill `movil`.

Objetivo: una primera versión **pequeña, publicable y útil**. La mayoría de apps que no salen nunca mueren por alcance.

Lee `REGLAS.md` de la skill `movil`, `plan/REQUISITOS.md` y `plan/PERFIL.md`. Plantilla: `plantillas/ALCANCE.md` de la skill `movil`.

## Flujo

1. **El objetivo del MVP en una frase**: qué tiene que demostrar (p. ej. «que 20 personas la usen una semana seguida»).
2. **Recorta.** Para cada requisito «Debe», pregunta: «Si la app saliera sin esto, ¿seguiría sirviendo para lo esencial?». Si la respuesta es sí, va fuera. Los «Debería» y «Podría» van fuera por defecto.
3. **Recortes que casi siempre ayudan** (propónlos si aplican):
   - Una sola plataforma primero si solo tiene un tipo de móvil (normalmente la suya).
   - Sin cuentas si se puede: datos en el móvil. Las cuentas traen backend, borrar cuenta, privacidad y login con Apple.
   - Un idioma.
   - Solo móvil (iPhone, sin iPad: `supportsTablet: false`).
   - Sin pagos en la v1; se añaden cuando haya usuarios.
   - Sin panel de administración: se edita en la consola del backend.
4. **Añade lo obligatorio de tienda** (sección de la plantilla): privacidad, soporte, borrar cuenta, login con Apple si hay otros logins sociales, reportar/bloquear si hay contenido de usuarios, textos de permisos, cuenta demo.
5. **Riesgos**: los 2-3 que pueden tumbar el plan (permiso delicado, temática sensible, dependencia de un servicio externo, falta de testers) y qué hacer.
6. Repasa el resultado con ella y pide un «sí, esto es la v1».

## Reglas duras

- Si hay duda, fuera. Cuesta poco añadirlo en una actualización.
- Nada entra en el MVP sin ID de requisito.
- Guarda lo que sale fuera con «cuándo lo reconsideramos»: así no se siente que se pierde.

## Salida

1. `plan/ALCANCE.md`.
2. `plan/ESTADO.md`: fase → «hitos».
3. Commit: `plan: alcance del MVP`.
4. Siguiente: **movil-hitos**.
