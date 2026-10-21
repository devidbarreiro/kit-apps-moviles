---
name: movil-hitos
description: Divide el MVP de una app móvil en hitos pequeños (H0, H1, H2…), cada uno con objetivo, requisitos cubiertos y criterios de "hecho" que se comprueban en el móvil, más los hitos de prueba real, tiendas y lanzamiento. Deja plan/HOJA_DE_RUTA.md. Úsala cuando digan "hazme un plan", "hitos", "milestones", "hoja de ruta", "roadmap", "en qué orden lo hago", o `/movil-hitos`.
---

# movil-hitos

> **Archivos compartidos**: `REGLAS.md`, `plantillas/` y `referencias/` están en la skill `movil`, carpeta hermana de esta (`../movil/`). Si no la encuentras, búscala (`**/movil/REGLAS.md`). Si en el proyecto no existe `plan/ESTADO.md`, propón empezar por la skill `movil`.

Objetivo: una hoja de ruta en la que **cada hito termina con algo que se ve en el móvil**.

Lee `REGLAS.md` de la skill `movil`, `plan/ALCANCE.md`, `plan/REQUISITOS.md` y `plan/PERFIL.md`. Plantilla: `plantillas/HOJA_DE_RUTA.md`.

## Cómo partir

1. **H0 · Preparación** siempre primero: herramientas, proyecto Expo creado, app en blanco abierta en su móvil, git.
2. **H1 · El esqueleto navegable**: todas las pantallas del MVP con datos falsos y navegación entre ellas. Le permite «tocar» la app muy pronto y cambiar de idea barato.
3. **H2…Hn · Una funcionalidad de punta a punta por hito**, en orden de valor: primero lo que hace única a la app. Cada hito cubre requisitos concretos (IDs).
4. Si hay backend: un hito propio justo antes del primero que lo necesite («datos reales y cuentas»), con RLS desde el principio.
5. Lo «obligatorio de tienda» (borrar cuenta, ajustes, privacidad, soporte) en un hito propio antes de la prueba real; no lo dejes para el final.
6. **HP · Prueba real**: build instalable (TestFlight / prueba interna de Google) usado por 2-3 personas una semana.
7. **HT · Tiendas**: fichas, capturas, privacidad. Si es cuenta personal nueva de Google, la **prueba cerrada de 14 días** empieza aquí: arráncala cuanto antes, puede solaparse con los últimos arreglos.
8. **HL · Lanzamiento**.

## Reglas

- Hitos de 1-2 semanas para alguien que dedica pocas horas. Si un hito tiene más de 5 criterios, pártelo.
- Criterios de «hecho» observables por ella, sin jerga: «Al pulsar Guardar, la nota aparece en la lista y sigue ahí al cerrar y abrir la app».
- Cada requisito del MVP aparece en algún hito. Comprueba la cobertura y dilo («los 9 requisitos del MVP están repartidos»).
- Marca qué hitos requieren *development build* (librerías nativas: mapas, notificaciones push, login con Google/Apple, compras, widgets): conviene pasar a development build en el hito anterior.

## Salida

1. `plan/HOJA_DE_RUTA.md`.
2. `plan/ESTADO.md`: fase → «preparación», hito actual H0, progreso.
3. Commit: `plan: hoja de ruta`.
4. Siguiente: **movil-preparar**.
