---
name: movil-entrevista
description: Entrevista inicial antes de crear una app móvil - si trabaja en Mac o Windows, qué móviles tiene para probar, a qué tiendas quiere ir, presupuesto, cuentas (Apple Developer, Google Play, Expo) y experiencia. Deja el resultado en plan/PERFIL.md y deduce qué se puede hacer y qué no. Úsala al empezar una app nueva, o cuando digan "tengo Windows, ¿puedo hacer app de iPhone?", "¿qué necesito para hacer una app?", "¿cuánto cuesta publicar?", o `/movil-entrevista`.
---

# movil-entrevista

> **Archivos compartidos**: `REGLAS.md`, `plantillas/` y `referencias/` están en la skill `movil`, carpeta hermana de esta (`../movil/`). Si no la encuentras ahí, búscala en `~/.claude/skills/movil/` o en `~/.claude/plugins/` (`**/movil/REGLAS.md`). Si en el proyecto no existe `plan/ESTADO.md`, propón empezar por la skill `movil`.

Objetivo: conocer el punto de partida para no planear algo imposible. 10 minutos, una pregunta cada vez.

Lee `REGLAS.md` de la skill `movil`. Plantilla: `plantillas/PERFIL.md` de la skill `movil`.

## Preguntas (en este orden, con opciones)

1. **Ordenador**: ¿Mac o Windows? (Linux funciona como Windows a efectos de iOS, pero el kit está pensado para Mac y Windows.) Si es Mac, ¿Apple Silicon (M1 o posterior) o Intel? ¿Cuánto espacio libre tiene? (Comprueba tú: `df -h ~` en Mac/Linux; en Windows, `Get-PSDrive C` en PowerShell.)
2. **Móviles**: ¿Tiene iPhone? ¿Android? ¿Puede pedir prestado el otro?
3. **Tiendas**: ¿iOS, Android o las dos? ¿Cuál primero?
4. **Cuentas**: ¿Tiene ya cuenta de Apple Developer, Google Play Console, Expo y GitHub? ¿Las abrirá como **persona** o como **empresa**?
5. **Presupuesto**: ¿Puede pagar 99 €/año (Apple) y 25 $ una vez (Google)? ¿Algo al mes para servicios?
6. **Experiencia y tiempo**: ¿Ha hecho algo técnico antes? ¿Cuántas horas a la semana? ¿Hay fecha objetivo?

## Qué deducir (y explicárselo)

| Situación | Consecuencia |
|---|---|
| **Mac (Apple Silicon)** | Puede todo: simulador de iPhone (Xcode), emulador de Android (Android Studio), builds en la nube o en local. Xcode ocupa 30-40 GB: si hay poco espacio, empieza sin él (Expo Go + móvil real) y deja Xcode para cuando haga falta. |
| **Windows (o Linux)** | Android: completo (emulador opcional con Android Studio). **iOS: sin simulador y sin compilar en local.** Se compila en la nube con **EAS Build** y se prueba en un **iPhone físico**: primero con Expo Go; cuando haga falta código nativo, con un *development build* (requiere Apple Developer de pago para registrar el iPhone) o por TestFlight. Subir a la App Store se hace con `eas submit`, sin Mac. |
| **Sin iPhone** | Puede desarrollar para iOS, pero no probarlo de verdad: busca a alguien con iPhone para TestFlight antes de publicar. Apple revisa en un iPhone real. |
| **Sin Android** | Igual: un emulador sirve para desarrollar, pero la prueba cerrada de Google exige testers reales. |
| **Cuenta personal nueva de Google Play** | Antes de publicar: **prueba cerrada con al menos 12 testers durante 14 días seguidos**. Hay que tener los 12 contactos pensados desde ya. Con cuenta de **empresa** no se exige, pero pide número D-U-N-S (gratis, tarda días o semanas). |
| **Cuenta de Apple como empresa** | Pide D-U-N-S y que la empresa exista legalmente. Como persona, en la tienda sale su nombre real como vendedor. |
| **Sin presupuesto para Apple** | Puede desarrollar y probar con Expo Go gratis; los 99 € solo hacen falta para TestFlight, development builds en iPhone y publicar. Decide cuándo pagarlos (no antes de tener el MVP usable). |
| **Poco tiempo** | MVP más pequeño y hitos de una semana. |

Costes de referencia (comprueba los actuales en las webs oficiales y díselo):
- Apple Developer Program: 99 €/año (https://developer.apple.com/programs/).
- Google Play Console: 25 $ pago único (https://play.google.com/console/signup).
- Expo / EAS: plan gratuito con un cupo mensual de builds en la nube; suficiente para empezar (https://expo.dev/pricing).
- Backend (p. ej. Supabase): plan gratuito para empezar; ojo, los proyectos gratuitos se **pausan tras unos días sin uso** (https://supabase.com/pricing).
- Dominio para la web de privacidad/soporte: opcional (se puede usar una web estática gratuita).

## Salida

1. `plan/PERFIL.md` rellenado, con la sección «Consecuencias para el plan».
2. Lista corta de **cuentas a abrir ya** (las verificaciones tardan días): Expo (gratis, ahora), GitHub (gratis, ahora), Google Play y Apple cuando decida pagar. No las abras tú: dale el enlace y qué elegir.
3. `plan/ESTADO.md`: fase → «idea», decisiones tomadas (plataformas, persona/empresa).
4. Commit: `plan: perfil inicial`.
5. Siguiente: **movil-idea**.
