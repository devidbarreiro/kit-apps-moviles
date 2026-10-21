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
7. **Testers de Android**: si quiere Google Play con cuenta personal nueva, ¿conoce a unas **15 personas con móvil Android** dispuestas a instalar una versión de prueba y no borrarla en dos semanas? (Google pide 12 durante 14 días; se piden 15 porque alguno se cae.)

## Decisiones tempranas (tómalas aquí, con ella)

### A. Expo Go o pagar Apple pronto

Expo Go es la app gratuita que abre tu proyecto en el móvil escaneando un QR. Tiene una trampa: **la Expo Go de las tiendas solo abre proyectos del SDK de Expo más reciente**. Cuando Expo saca un SDK nuevo (varias veces al año), la Expo Go del móvil se actualiza y deja de abrir el proyecto hasta que lo actualices también. En Android se puede instalar una Expo Go antigua; **en iPhone no** (Apple no permite instalar versiones viejas).

Comprueba hoy qué SDK abre la Expo Go de la tienda (https://expo.dev/go y el *changelog* de https://expo.dev/changelog) y explícale su caso:

| Su caso | Qué significa | Recomendación |
|---|---|---|
| **Windows (o Mac sin Xcode) + iPhone, sin pagar los 99 €** | Solo puede probar con Expo Go. Si sale un SDK nuevo a mitad del proyecto, tendrá que actualizar el proyecto (a veces rompe cosas) o pagar Apple para pasar a *development build*. | Aceptable para empezar si el MVP no tiene nada nativo (ver `movil-alcance`). Avisa ya de que antes de publicar pagará igualmente. |
| **Android** | Expo Go o un *development build* (APK) gratis, sin restricciones. | Sin problema. |
| **Mac con Xcode** | El simulador de iPhone abre cualquier SDK; puede trabajar sin pagar hasta publicar. | Sin problema si tiene espacio para Xcode. |
| **Va a pagar Apple pronto** | *Development build* en su iPhone desde el principio; las actualizaciones de Expo Go dejan de importar. | La opción más tranquila si el MVP tiene algo nativo. |

Apunta en `plan/PERFIL.md` → «SDK y Expo Go»: el **SDK fijado** (el de `expo` en `package.json`), cómo probará (Expo Go / development build / simulador) y cuándo piensa pagar Apple. No cambies de SDK sin decidirlo con ella.

### B. ¿Qué tienda primero?

- Si quiere Android con **cuenta personal nueva** y **no tiene ~15 personas con Android** para la prueba cerrada, recomienda **publicar primero en iOS** y preparar Android en paralelo (la prueba cerrada se puede abrir más tarde).
- Si solo tiene un tipo de móvil, la primera tienda suele ser la de su móvil.

### C. Persona o empresa

Por defecto, **persona** (más rápido; no pide D-U-N-S). Empresa solo si ya existe legalmente y quiere que salga su nombre como vendedor. En la UE, las dos tiendas preguntan si es **comerciante** (DSA): si la app es gratis y sin actividad comercial, normalmente «no comerciante»; si cobra o es parte de su negocio, «comerciante» (su dirección y teléfono se verán en la ficha). Esto no es asesoramiento legal: que lo confirme en la ayuda de cada tienda (`referencias/tiendas.md`).

## Qué deducir (y explicárselo)

| Situación | Consecuencia |
|---|---|
| **Mac (Apple Silicon)** | Puede todo: simulador de iPhone (Xcode), emulador de Android (Android Studio), builds en la nube o en local. Xcode ocupa 30-40 GB: si hay poco espacio, empieza sin él (Expo Go + móvil real) y deja Xcode para cuando haga falta. |
| **Windows (o Linux)** | Android: completo (emulador opcional con Android Studio). **iOS: sin simulador y sin compilar en local.** Se compila en la nube con **EAS Build** y se prueba en un **iPhone físico**: primero con Expo Go; cuando haga falta código nativo, con un *development build* (requiere Apple Developer de pago para registrar el iPhone) o por TestFlight. Subir a la App Store se hace con `eas submit`, sin Mac. |
| **Sin iPhone** | Puede desarrollar para iOS, pero no probarlo de verdad: busca a alguien con iPhone para TestFlight antes de publicar. Apple revisa en un iPhone real. |
| **Sin Android** | Igual: un emulador sirve para desarrollar, pero la prueba cerrada de Google exige testers reales. |
| **Cuenta personal nueva de Google Play** | Antes de publicar: **prueba cerrada con al menos 12 testers durante 14 días seguidos**. Hay que tener los 12 contactos pensados desde ya. Con cuenta de **empresa** no se exige, pero pide número D-U-N-S (gratis, tarda días o semanas). |
| **Cuenta de Apple como empresa** | Pide D-U-N-S y que la empresa exista legalmente. Como persona, en la tienda sale su nombre real como vendedor. |
| **Sin presupuesto para Apple** | Puede desarrollar y probar con Expo Go gratis; los 99 € solo hacen falta para TestFlight, development builds en iPhone y publicar. Ojo con la trampa de Expo Go (decisión A) y con las funciones nativas del MVP (`movil-alcance`), que adelantan el pago. |
| **Poco tiempo** | MVP más pequeño y hitos de una semana. |

Costes de referencia (verificados 2026-10; comprueba los actuales en `referencias/tiendas.md` → «Requisitos con fecha» y díselo):
- Apple Developer Program: 99 USD/año, en euros según el país al darse de alta (https://developer.apple.com/programs/enroll/).
- Google Play Console: 25 USD, pago único (https://support.google.com/googleplay/android-developer/answer/6112435).
- Expo / EAS: plan gratuito con 15 builds de Android y 15 de iOS al mes, en cola de baja prioridad (https://expo.dev/pricing).
- Claude: Claude Code necesita un plan de pago (Pro o superior) (https://claude.com/pricing).
- Backend (p. ej. Supabase): plan gratuito para empezar; ojo, los proyectos gratuitos se **pausan tras unos días sin uso** (https://supabase.com/pricing).
- Dominio para la web de privacidad/soporte: opcional (se puede usar una web estática gratuita).

## Salida

1. `plan/PERFIL.md` rellenado, con la sección «Consecuencias para el plan».
2. Lista corta de **cuentas a abrir ya** (las verificaciones tardan días): Expo (gratis, ahora), GitHub (gratis, ahora), Google Play y Apple cuando decida pagar. No las abras tú: dale el enlace y qué elegir.
3. `plan/ESTADO.md`: fase → «idea», decisiones tomadas (plataformas y cuál primero, persona/empresa, SDK fijado, Expo Go o development build, cuándo pagar Apple).
4. Commit: `plan: perfil inicial`.
5. Siguiente: **movil-idea**.
