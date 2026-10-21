# Stack recomendado

_Verificado 2026-10: comprueba en la doc de Expo que cada recomendación sigue vigente._

Lo que hemos usado en varias apps publicadas con Expo. **Las versiones las fija el SDK**: instala siempre con `npx expo install` y consulta la doc de la versión del proyecto.

## Base (siempre)

| Pieza | Qué usar | Por qué |
|---|---|---|
| Framework | **Expo** (SDK más reciente estable) + React Native + TypeScript estricto | Una base de código para iOS y Android; builds y publicación sin Mac gracias a EAS |
| Navegación | **expo-router** (rutas por archivos en `app/`) | Oficial, enlaces profundos gratis, rutas tipadas |
| Builds y tiendas | **EAS Build / Submit / Update** | Compila en la nube (iOS desde Windows), gestiona certificados, sube a las tiendas, actualizaciones al momento |
| Desarrollo | **Expo Go** al principio → **expo-dev-client** en cuanto haya algo nativo | |
| Calidad | `tsc --noEmit`, `expo-doctor`, `eslint-config-expo` (`npx expo lint`) | |
| Tests | **jest-expo** (el preset oficial de Jest para Expo) sobre funciones puras y, si hace falta, componentes | Rápidos, sin móvil; guía: https://docs.expo.dev/develop/unit-testing/ |
| Datos | **En el móvil por defecto**: `expo-sqlite` (datos con estructura, listas, búsquedas) o AsyncStorage (preferencias y poca cosa); `expo-secure-store` para tokens | Sin servidor: más simple, más privado, más fácil de aprobar. Guía: https://docs.expo.dev/develop/user-interface/store-data/ |

## SDK de Expo (preferir siempre a alternativas)

| Necesidad | Módulo |
|---|---|
| Imágenes | `expo-image` (caché, placeholders) |
| Fuentes | `expo-font` / `@expo-google-fonts/*` |
| Splash | `expo-splash-screen` |
| Iconos | `@expo/vector-icons` (incluido) o `expo-symbols` (SF Symbols en iOS) |
| Guardar secretos pequeños (tokens) | `expo-secure-store` |
| Base de datos local | `expo-sqlite` |
| Archivos | `expo-file-system` |
| Cámara / escanear QR | `expo-camera` |
| Elegir fotos | `expo-image-picker` (usa el selector del sistema: no necesita permiso de fotos completo) |
| Ubicación | `expo-location` (+ `expo-task-manager` si es en segundo plano) |
| Notificaciones | `expo-notifications` + `expo-device` (push vía servicio de Expo) |
| Vibración | `expo-haptics` |
| Audio / vídeo | `expo-audio` / `expo-video` |
| Compartir | `expo-sharing` |
| Abrir enlaces | `expo-linking`, `expo-web-browser` |
| Login con Apple | `expo-apple-authentication` |
| Aleatoriedad / hashes | `expo-crypto` |
| Actualizaciones al momento | `expo-updates` |
| Localización (idioma, región) | `expo-localization` |

## Comunidad, muy usadas y mantenidas

| Necesidad | Librería | Nota |
|---|---|---|
| Almacenamiento clave-valor | `@react-native-async-storage/async-storage` | |
| Estado global | `zustand` (+ `persist`) | Simple; versiona el estado persistido y valídalo al cargar |
| Datos remotos y caché | `@tanstack/react-query` | Con *persister* da «offline» de lo ya consultado |
| Animaciones | `react-native-reanimated` + `react-native-gesture-handler` | Incluidas en la plantilla |
| Hojas inferiores | `@gorhom/bottom-sheet` | |
| SVG | `react-native-svg` | |
| Áreas seguras | `react-native-safe-area-context` | |
| Estilos tipo Tailwind (opcional) | `nativewind` | Útil, pero tiene trampas (ver `trampas.md`). Si la persona no lo necesita, `StyleSheet` basta |
| Componentes base (opcional) | react-native-reusables (estilo shadcn para RN) | shadcn/ui de web **no** sirve en nativo |
| Backend (solo si hace falta) | `@supabase/supabase-js` + `react-native-url-polyfill` | Solo con cuentas, sincronización entre dispositivos o datos compartidos, justificado en `REQUISITOS.md`. Ver `movil-backend` |
| Login con Google | `@react-native-google-signin/google-signin` | Necesita development build y 3 tipos de cliente OAuth |
| Mapas | `react-native-maps` (sencillo) · `@maplibre/maplibre-react-native` (gratuito, estilos propios) · `@rnmapbox/maps` | Los dos últimos necesitan development build |
| Widgets | iOS: `@bacons/apple-targets` · Android: `react-native-android-widget` | Avanzado; development build |
| Informes de errores | `@sentry/react-native` | Declararlo en privacidad antes de activarlo |
| Compras integradas | `react-native-purchases` (RevenueCat) o `expo-iap` | Comprueba la recomendación actual en docs.expo.dev |

## Decisiones de arquitectura que nos funcionaron

- **Lógica pura separada de la interfaz**: reglas y cálculos en funciones/reductores sin React, con tests. Cambiar de «solo en el móvil» a «online» fue cambiar de dónde salen los datos, no la lógica.
- **Interfaz para el backend** (p. ej. `Transport` con `crear`, `leer`, `publicar`, `suscribir`) y una versión falsa en memoria para tests.
- **Resultados explícitos** `{ ok: true, value } | { ok: false, error: { kind: 'red' | 'no-encontrado' | … } }` en vez de lanzar errores por todas partes.
- **Backend opcional**: si faltan las variables, la app funciona en modo local y oculta lo online (en desarrollo); en producción, error claro.
- **Archivos por plataforma** (`Mapa.native.tsx` / `Mapa.web.tsx`) cuando algo solo existe en una.
- **Módulos nativos con «guardia»**: si el módulo no está (Expo Go o binario viejo), el botón no aparece y la pantalla sigue funcionando.
- **`app/` fino**: cada archivo de ruta solo importa la pantalla de `src/features/…`.
- **Datos antes que código**: lo que puede venir de la base de datos (catálogos, avisos, textos) llega a todos al instante sin revisión.
- **Recursos generados por script** (iconos, sonidos, capturas redimensionadas): reproducibles y sin problemas de licencia.
- **Notas por sesión** en `docs/` o `plan/`: qué se hizo, cómo probarlo, qué falta. Imprescindible para trabajar con IA en varias sesiones.
