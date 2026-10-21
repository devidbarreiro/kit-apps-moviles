# Trampas técnicas conocidas

Errores reales que nos encontramos en apps Expo publicadas, con su arreglo. Antes de aplicar un arreglo, comprueba que sigue vigente en la versión del SDK del proyecto.

## Configuración y variables

- **Los builds de EAS no ven tu `.env`** (EAS respeta `.gitignore`). La app sale «vacía» sin error. → Crea las variables en EAS (`eas env:set`; antes `eas env:create`) en cada entorno, y un `src/config.ts` que **lance un error con el nombre de la variable** si falta en producción (y si está a medias: URL sin clave).
- **`EXPO_PUBLIC_*` acaban dentro de la app.** Márcalas como `plaintext` en EAS; marcarlas como secretas da falsa seguridad. Secretos de verdad solo en el servidor.
- **Opciones de *config plugins* se incrustan en el manifiesto de la app.** Nunca pongas un token secreto en la configuración de un plugin; pásalo como variable de entorno del build si la librería lo permite.
- **Ficheros de configuración de Google** (`google-services.json`): fuera del repo, como variable de EAS tipo fichero (`--type file`).
- **Revisa la configuración final** antes de cada build: `npx expo config --type public` o `--type introspect` (permisos y Info.plist resultantes).

## Permisos

- **Los plugins añaden permisos que no usas.** `expo-audio` activa micrófono y audio en segundo plano; `expo-image-picker` añade micrófono en Android; `expo-location` puede pedir textos de movimiento. Un permiso injustificado es motivo de rechazo. → Desactívalos en las opciones del plugin (`microphonePermission: false`, `recordAudioAndroid: false`, `enableBackgroundPlayback: false`…) y en Android con `android.blockedPermissions`.
- **Al revés: Apple rechaza el binario si una librería enlaza una API sin su texto de uso** (p. ej. `NSMotionUsageDescription` con `expo-location`, error ITMS-90683), aunque no la uses. → Pon un texto honesto.
- **Textos de permisos**: concretos, con el nombre de la app, en el idioma de la app. Si no los defines, algunos plugins ponen un texto genérico en inglés.
- **Pide el permiso cuando se usa**, con una pantalla previa neutra («Continuar», no «Dar permiso»: Apple lo considera presionar, guideline 5.1.1(iv)). Si lo deniega y `canAskAgain` es falso, botón «Abrir Ajustes» (`Linking.openSettings()`) y siempre una alternativa manual.
- **Notificaciones en Android 13+**: crea el canal **antes** de pedir permiso; sin canal, el diálogo no sale.
- **Ubicación en segundo plano**: pide primero «mientras se usa» y «siempre» más tarde y en contexto; activa el indicador azul de iOS; notificación clara del servicio en primer plano en Android. Se revisa con lupa.

## NativeWind

- **`style` en forma de función en `Pressable` se descarta en silencio** (`style={({pressed}) => …}`). → Pon los estilos en un `View` hijo o usa `style` como objeto.
- **Clase de sombra condicional** (`className={activa ? 'shadow-sm' : ''}`) hace que NativeWind reconstruya el componente; fuera del contenedor de navegación (p. ej. una hoja sobre un mapa) revienta con *«Couldn't find a navigation context»*. → Sombra condicional con un objeto de estilo:
  ```ts
  const SOMBRA = { shadowColor: '#000', shadowOffset: { width: 0, height: 1 }, shadowOpacity: 0.08, shadowRadius: 2, elevation: 2 };
  <Pressable style={activa ? SOMBRA : undefined} />
  ```
- **`overflow: 'hidden'` recorta la propia sombra en iOS.** Sombra en un contenedor exterior, recorte en uno interior.

## Teclado

- **`KeyboardAvoidingView` bajo una cabecera nativa deja el campo tapado** por la altura de la cabecera. → `keyboardVerticalOffset={useHeaderHeight()}` (de `@react-navigation/elements`; solo dentro de un navegador).
- **Android borde a borde** (por defecto en versiones recientes) ya no encoge la ventana: `behavior={Platform.OS === 'ios' ? 'padding' : undefined}` deja campos tapados. → `behavior="padding"` en las dos, centralizado en una constante.
- **Chat / lista sobre un campo fijo**: al aparecer el teclado el último mensaje queda escondido. → Escucha el alto de la lista con `onLayout` y haz `scrollTo` lo que encoja.

## Navegación (expo-router)

- **Acciones pendientes entre pantallas** (una pantalla deja en el estado «abre X» y navega a otra): si la pantalla destino aplica la acción antes de estar visible, se «consume» en el vacío. → Aplícala solo cuando `useIsFocused()` sea `true`, y márcala como consumida después.
- **`router.back()` sin historial** (entrada por enlace profundo) lanza error. → Helper `volver()`: `router.canGoBack() ? router.back() : router.replace('/')`.
- **Android: el botón atrás con una hoja/modal abierto sale de la pantalla.** → Interceptarlo y cerrar primero la hoja.
- **Android `Alert` solo muestra 3 botones.** Más opciones → hoja propia.
- **Pantalla de bienvenida que reaparece** a usuarios que ya la vieron: el primer render ocurre antes de leer AsyncStorage. → Estado de tres valores `'cargando' | 'mostrar' | 'hecho'`.

## Estado persistido

- Versiona el estado guardado (`version` + `migrate` en zustand `persist`) y **valida su forma al cargar**: el disco es entrada externa. Si no cuadra, reinicia ese trozo antes que arrancar una app rota.
- Guarda las preferencias (p. ej. «ya vio la bienvenida») en un almacén separado del resto, para que una migración no las borre.
- **No cambies las claves de almacenamiento al renombrar la app**: borrarías los datos de los usuarios.

## Módulos nativos y Expo Go

- Importar estáticamente una librería nativa **revienta en Expo Go** al evaluar el archivo. → Comprueba que el módulo existe y cárgalo con `require()` perezoso dentro de `try/catch`, o usa una alternativa sin nativo en desarrollo.
- En builds de producción, los módulos se cargan bajo demanda: una inicialización (p. ej. el token de un mapa) puesta en un archivo que todavía no se ha cargado **no se ha ejecutado**. → Inicializa en un punto de entrada común que todos usen.
- Los cambios de estilos de algunos mapas nativos no se ven con recarga rápida: reinicia la app.

## Builds (EAS)

- **Números de build**: `cli.appVersionSource: "remote"` + `autoIncrement` y **ninguno** en `app.json`. Un build fallido (o rechazado por cupo) ya gastó su número: los huecos dan igual. `eas build:version:get` para consultarlo.
- **Capacidades nuevas** (Iniciar sesión con Apple, push): EAS solo las sincroniza en un build **interactivo**. Si falla la firma, lanza uno desde una terminal normal.
- **`eas submit` no interactivo** necesita el `ascAppId` en `eas.json` → `submit.production.ios`.
- **Un plugin de informes de errores sin configurar** (p. ej. Sentry sin cuenta) rompe el build en el paso de bundle sin mencionarlo. → Quítalo hasta tener cuenta, o `SENTRY_DISABLE_AUTO_UPLOAD=true`.
- **Subida lenta a EAS**: revisa `.easignore`/`.gitignore`; carpetas grandes (vídeos, datos) se suben con el proyecto.
- **Xcode nuevo vs SDK de Expo**: un Xcode más nuevo que el soportado puede no compilar módulos de Expo. → Compila en la nube de EAS (usa la imagen compatible) o instala la versión de Xcode indicada. Parchear Expo (patch-package) es último recurso: nunca cambies firmas públicas ni el modo de lenguaje de Swift (compila, pero revienta al abrir).
- **Si la nube falla rápido en «Run fastlane»** y tienes Mac: `eas build --local`.
- **No subas `ios/` ni `android/`** (se generan con `npx expo prebuild`): el proyecto queda limpio y los cambios nativos viven en `app.json` y plugins.

## Login con Google

Tres tipos de cliente OAuth en Google Cloud:
1. **Web**: su ID va en la app (`webClientId`) y en el proveedor de auth (Supabase), con su secreto solo en el panel del proveedor.
2. **iOS**: su ID va en la app y en el *URL scheme* (el ID invertido).
3. **Android**: **uno por cada huella SHA-1**: la de depuración, la de la *keystore* de subida de EAS (`eas credentials`) y la de firma de Google Play (Play Console → Integridad de la app). No van en la app. Si falta uno → `DEVELOPER_ERROR`.

Además: en Supabase, los IDs separados por comas **con el web primero**; pantalla de consentimiento en «En producción»; para probar en emulador, imagen con **Google Play**. Ofrece **Iniciar sesión con Apple** en iOS (obligatorio si hay Google) y colócalo primero.

## Login con Apple

- Apple da el nombre **solo la primera vez**: guárdalo en ese momento.
- Con «Ocultar mi correo», los correos solo llegan si el dominio remitente está registrado en el portal de Apple (Sign in with Apple for Email Communication).
- Al borrar la cuenta, revoca el token de Apple.

## Supabase

- **RLS en todas las tablas**, vistas con `security_invoker`, funciones `security definer` con `search_path` fijo. Las funciones nuevas son ejecutables por la clave pública por defecto: `revoke execute … from anon, authenticated` en las de servidor.
- **`returns tabla` devuelve una fila de nulos si no encuentra nada**; el cliente lo ve como éxito. → `returns setof tabla` y comprueba la lista.
- **Realtime**: usa Broadcast si no quieres dar lectura de la tabla a la clave pública. Los mensajes no se reenvían: vuelve a leer al volver del segundo plano o al reconectar. Pasa siempre el *callback* de estado a `.subscribe()` y reconecta ante `TIMED_OUT`, `CHANNEL_ERROR` o `CLOSED`.
- **Proyecto gratuito pausado** tras unos días sin uso: el revisor ve la app rota. → Plan de pago o tarea diaria que haga una consulta real.
- **Correo integrado** con límite muy bajo: SMTP propio antes de abrir el registro.
- **Enlaces de recuperar contraseña** que fallan («Verify requires a verification type»): plantilla con `{{ .TokenHash }}` y `type=recovery`, y el `scheme` de la app en Redirect URLs.
- **Local ≠ nube**: el entorno local puede tener otros permisos que producción. Compara en las dos direcciones; lo peligroso es que local sea **más** permisivo.
- **Sin conexión, el token caduca**: no cierres la sesión por un fallo de refresco sin mirar el motivo.
- Migraciones **compatibles con la versión publicada** e idempotentes; comprueba su efecto, no solo que «terminaron».

## Varios

- **Rutas SVG**: `react-native-svg` en iOS puede colgarse con datos de ruta que el navegador acepta. Genera los `d` con un helper y pruébalos.
- **Texto de desarrollador** (nombres de variables, errores en inglés) que llega a una pantalla: revísalo antes de cada envío.
- **El simulador de iPhone está en San Francisco** por defecto: simula ubicación con `xcrun simctl location booted set <lat>,<lng>`.
- **Aleatoriedad seria** (juegos, sorteos): `expo-crypto.getRandomBytes`, no `Math.random`.
