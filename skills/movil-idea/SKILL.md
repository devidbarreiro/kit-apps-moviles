---
name: movil-idea
description: Convierte la idea de una app móvil en requisitos claros - para quién es, qué problema resuelve, qué tiene que poder hacer el usuario, qué datos maneja y cómo gana dinero. Deja plan/REQUISITOS.md y un primer plan/PRIVACIDAD.md. Úsala cuando digan "tengo una idea de app", "ayúdame a definir la app", "qué tiene que hacer mi app", "requisitos", o `/movil-idea`.
---

# movil-idea

> **Archivos compartidos**: `REGLAS.md`, `plantillas/` y `referencias/` están en la skill `movil`, carpeta hermana de esta (`../movil/`). Si no la encuentras ahí, búscala en `~/.claude/skills/movil/` o en `~/.claude/plugins/` (`**/movil/REGLAS.md`). Si en el proyecto no existe `plan/ESTADO.md`, propón empezar por la skill `movil`.

Objetivo: pasar de «tengo una idea» a una lista de requisitos verificables. Sin hablar todavía de tecnología.

Lee `REGLAS.md` de la skill `movil`. Plantillas: `REQUISITOS.md` y `PRIVACIDAD.md` en `plantillas/` de la skill `movil`.

## Flujo

1. **Cuéntamela como a un amigo.** Deja que la explique libremente. Después resume en una frase con el formato «Para <quién> que <problema>, <app> permite <resultado>» y pide que lo confirme o corrija.
2. **Usuarios.** ¿Quién la usará? ¿Hay varios tipos (p. ej. cliente y negocio)? ¿Cada cuánto? Si hay dos tipos de usuario muy distintos, avisa: casi duplica el trabajo; propón empezar por uno.
3. **Un día con la app.** Pide que narre paso a paso qué hace un usuario desde que abre la app. De ahí salen los requisitos.
4. **Alternativas.** ¿Qué usa hoy esa gente (otra app, WhatsApp, papel)? ¿Por qué cambiaría? Si existe una app casi igual, dilo con tacto: la diferencia tiene que estar clara también para el revisor de Apple (rechazan apps «copia» o de poco valor).
5. **Requisitos.** Escríbelos tú en la tabla, uno por fila, empezando por «El usuario puede…», con ID (R01, R02…) y prioridad Debe/Debería/Podría. Léeselos y ajusta.
6. **No funcionales.** Pregunta solo lo que cambia el diseño: ¿sin conexión?, ¿idiomas?, ¿cuentas de usuario?, ¿datos compartidos entre móviles o personas?, ¿ubicación, cámara, notificaciones?
7. **Datos.** Por cada requisito, ¿qué datos guarda o envía? Rellena `plan/PRIVACIDAD.md` con lo que sepas ya.
8. **Dinero.** ¿Gratis, pago único, suscripción, anuncios? Explica: si vende contenido o funciones *digitales* dentro de la app, Apple y Google obligan a usar sus compras integradas y cobran comisión; los servicios físicos (un corte de pelo, una entrega) no.

## Señales de alarma (díselas pronto)

- **Necesita cuentas + datos compartidos entre personas** → hace falta backend (ver `movil-backend`). Más trabajo, más privacidad que declarar.
- **Contenido creado por usuarios visible para otros** → la tienda exige reportar, bloquear y moderar.
- **Ubicación en segundo plano, salud, menores, dinero real, apuestas, alcohol** → revisión más dura; hay que justificar cada permiso. Hay temáticas que Apple directamente no acepta: compruébalo en las App Review Guidelines antes de seguir.
- **«Como Instagram/Uber pero…»** → alcance enorme; en `movil-alcance` habrá que recortar mucho.

## Salida

1. `plan/REQUISITOS.md` y `plan/PRIVACIDAD.md` (primer borrador).
2. `plan/ESTADO.md`: fase → «alcance», decisiones (cuentas sí/no, backend sí/no, modelo de dinero).
3. Commit: `plan: requisitos`.
4. Siguiente: **movil-alcance**.
