# Cambios

Formato: una sección por versión, lo más nuevo arriba. Las versiones siguen `MAJOR.MINOR.PATCH` (el número también está en `.claude-plugin/plugin.json`).

## 0.1.0 · 2026-10 · en pruebas

Primera versión pública.

- 13 skills: `movil` (entrada) y `movil-entrevista`, `-idea`, `-alcance`, `-hitos`, `-preparar`, `-backend`, `-hito`, `-probar`, `-tienda`, `-publicar`, `-actualizar`, `-arreglar`.
- Plantillas de `plan/` (perfil, requisitos, alcance, hoja de ruta, estado, hito, privacidad, checklist de tiendas, política de privacidad), referencias (stack, trampas, tiendas) y lecciones.
- Orden de arranque: primero el proyecto Expo con `create-expo-app`, después `plan/` y git dentro (sin mover archivos ocultos).
- Hito **H0.5 · app en blanco en la tienda**: primer `eas build`, TestFlight interno y prueba interna de Google antes de programar funciones.
- Decisión temprana sobre Expo Go y los 99 USD de Apple; SDK de Expo fijado en `plan/PERFIL.md`; filtro de funciones nativas en el alcance.
- Sin backend por defecto (datos en el móvil); Supabase solo si hay cuentas, sincronización o datos compartidos.
- Tests con `jest-expo` (el preset oficial de Expo).
- Requisitos de las tiendas con fecha y fuente oficial («verificado 2026-10») en `skills/movil/referencias/tiendas.md`.
- README para principiantes: instalación como plugin, permisos de Claude Code, límites del plan, glosario, protocolo de errores, camino feliz y tiempos reales.
- Licencia MIT, plantillas de *issues* y web en GitHub Pages (`docs/`).
