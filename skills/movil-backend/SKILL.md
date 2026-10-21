---
name: movil-backend
description: Decide si una app móvil necesita backend y, si sí, lo monta con Supabase (Postgres, Auth, Storage) de forma segura - RLS en todas las tablas desde la primera migración, claves públicas en la app y secretas solo en el servidor, migraciones en git, login con email/Apple/Google y borrado de cuenta. Úsala cuando digan "necesito base de datos", "usuarios y login", "guardar datos en la nube", "Supabase", "sincronizar entre móviles", o `/movil-backend`.
---

# movil-backend

> **Archivos compartidos**: `REGLAS.md`, `plantillas/` y `referencias/` están en la skill `movil`, carpeta hermana de esta (`../movil/`). Si no la encuentras ahí, búscala en `~/.claude/skills/movil/` o en `~/.claude/plugins/` (`**/movil/REGLAS.md`). Si en el proyecto no existe `plan/ESTADO.md`, propón empezar por la skill `movil`.

Lee `REGLAS.md` y `referencias/trampas.md` (sección Supabase) de la skill `movil`, `plan/REQUISITOS.md` y `plan/ALCANCE.md`. Documentación: https://supabase.com/docs (guías de Expo/React Native, RLS, Auth).

## 1. ¿Hace falta?

| Necesita… | ¿Backend? |
|---|---|
| Guardar cosas solo para el propio usuario, en su móvil | **No.** AsyncStorage / `expo-sqlite` / `expo-file-system`. |
| Que los datos sobrevivan a cambiar de móvil | Sí (o copia de seguridad del sistema, más limitado). |
| Compartir datos entre personas o móviles | Sí. |
| Cuentas de usuario | Sí. |
| Notificaciones push enviadas desde «fuera» | Sí (algo que las envíe). |
| Usar una API con clave secreta (IA, pagos…) | Sí: la llamada va **desde el servidor**, nunca desde la app. |

**Por defecto, no.** Sin backend la app es más simple, más privada, más barata y más fácil de aprobar: los datos se guardan en el móvil con `expo-sqlite` (datos con estructura) o AsyncStorage (preferencias), según https://docs.expo.dev/develop/user-interface/store-data/. Solo se monta si hay **cuentas**, **sincronización entre dispositivos** o **datos compartidos entre personas** (o una clave secreta que no puede ir en la app), y el porqué tiene que estar escrito en `plan/REQUISITOS.md` → «¿Hace falta servidor?». Si no está justificado, dilo y vuelve.

Si hace falta, recomienda **Supabase** (Postgres gestionado, autenticación, almacenamiento de ficheros, funciones; plan gratuito). Alternativas si ella prefiere: Firebase, Convex. Este kit detalla Supabase.

## 2. Montaje

1. Ella crea la cuenta y el proyecto en https://supabase.com (región cercana a sus usuarios). Tú no creas cuentas.
2. En la app: `npx expo install @supabase/supabase-js @react-native-async-storage/async-storage react-native-url-polyfill` y sigue la guía oficial de Expo de Supabase para el cliente (sesión persistida en AsyncStorage; o `expo-secure-store` para el token si la guía lo recomienda).
3. Variables: `EXPO_PUBLIC_SUPABASE_URL` y la clave **pública** (`anon` / `publishable`) en `.env` y en EAS (`eas env:set`, ver `movil-preparar`). La clave `service_role` / `secret` **nunca** en la app ni en el repo.
4. Supabase CLI (`npx supabase init`, `npx supabase link`) para tener las **migraciones en git** (`supabase/migrations/`). Todo cambio de esquema es una migración, no clics en el panel. Supabase local necesita Docker: opcional; si no lo tiene, trabaja contra un proyecto de **desarrollo** separado del de producción.

## 3. Seguridad (no negociable)

- **RLS activado en todas las tablas** en la misma migración que las crea:
  ```sql
  create table public.notas (
    id uuid primary key default gen_random_uuid(),
    usuario_id uuid not null references auth.users(id) on delete cascade default auth.uid(),
    texto text not null check (char_length(texto) <= 2000),
    creada_en timestamptz not null default now()
  );
  alter table public.notas enable row level security;
  create policy "cada uno ve sus notas" on public.notas
    for select using (auth.uid() = usuario_id);
  create policy "cada uno crea sus notas" on public.notas
    for insert with check (auth.uid() = usuario_id);
  ```
  Tablas de referencia públicas: RLS activado igualmente, con política de solo lectura `using (true)`.
- **Vistas** con `security_invoker = true` (si no, se saltan la RLS).
- **Funciones** `security definer` solo si hace falta, con `set search_path = public`, y **revoca** el permiso de ejecución a `anon`/`authenticated` en las que sean solo de servidor (en Supabase las funciones nuevas quedan ejecutables por la clave pública por defecto).
- Revisa los avisos de **Security Advisor** del panel de Supabase en cada hito.
- Valida en la base de datos (`check`, `not null`, longitudes) además de en la app.
- Prueba la RLS: con la clave pública y un usuario A, intenta leer/escribir datos de B. Debe fallar.

## 4. Cuentas de usuario

- Empieza por **email + contraseña o enlace mágico**. Antes de abrir el registro a desconocidos, configura **SMTP propio** (p. ej. Resend, Postmark): el correo integrado de Supabase tiene un límite muy bajo y no sirve en producción.
- **Iniciar sesión con Apple** (`expo-apple-authentication`) es obligatorio en iOS si ofreces Google u otro login social. Apple da el nombre del usuario **solo la primera vez**: guárdalo en ese momento.
- **Google** (`@react-native-google-signin/google-signin`, necesita development build): ver `referencias/trampas.md` → «Login con Google». Resumen: un cliente OAuth **web** (va a Supabase y a la app), uno **iOS** y uno **Android por cada huella SHA-1** (debug, la de subida de EAS y la de firma de Play).
- **Borrar cuenta desde la app**: una función de servidor (Edge Function con la clave secreta) que borra los datos y el usuario. Obligatorio en ambas tiendas.
- Enlaces de correo (confirmación, recuperar contraseña) que abren la app: registra el `scheme` de la app en **Redirect URLs** de Supabase.

## 5. Planes gratuitos: no te duermas

Los proyectos gratuitos de Supabase **se pausan tras unos días sin actividad** (consulta el plazo actual). Si el revisor de Apple o Google abre la app con el backend pausado, la verá rota y la rechazará. Mientras la app esté publicada: plan de pago, o una tarea programada diaria (p. ej. GitHub Actions con `schedule`) que haga una consulta real (una RPC sencilla). Las tareas programadas de GitHub solo corren desde la rama principal y, en repositorios públicos, se desactivan tras 60 días sin actividad; comprueba de vez en cuando que siguen corriendo.

## 6. Privacidad

Por cada tabla, apunta en `plan/PRIVACIDAD.md` qué dato personal guarda, para qué y si está vinculado a la cuenta. Supabase (y Expo para push) son **encargados del tratamiento**, no «terceros con los que compartes»; menciónalos en la política.

## Salida

- Migraciones en `supabase/migrations/` con RLS, cliente en `src/lib/supabase.ts`, `src/config.ts` que falla si falta una variable.
- Prueba de RLS documentada en `plan/hitos/H<n>.md`.
- `plan/ESTADO.md` y `plan/PRIVACIDAD.md` actualizados; decisión «backend: Supabase» con el porqué.
- Commit: `backend: esquema inicial con RLS`.
