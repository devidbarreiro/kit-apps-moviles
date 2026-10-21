# Lecciones aprendidas publicando apps con Expo

Resumen de lo que aprendimos llevando varias apps propias (un mapa con cuentas y contenido social, un juego para varios jugadores sin cuentas y una app de ubicación en segundo plano) desde la idea hasta App Store y Google Play. El detalle técnico, para Claude, está en:

- [`skills/movil/referencias/trampas.md`](../skills/movil/referencias/trampas.md): errores técnicos y arreglos.
- [`skills/movil/referencias/tiendas.md`](../skills/movil/referencias/tiendas.md): cuentas, recursos, privacidad, rechazos reales.
- [`skills/movil/referencias/stack.md`](../skills/movil/referencias/stack.md): librerías y decisiones de arquitectura.

## Proceso

1. **El alcance mata más apps que el código.** Lo que no es imprescindible para la primera versión, fuera. Se añade con una actualización.
2. **Hitos que se ven en el móvil.** Cada hito termina con algo que puedes tocar. El primero, un esqueleto navegable con datos falsos: cambiar de idea ahí es barato.
3. **Escribe las reglas antes de programar.** Un documento con qué hace la app y cómo se comporta evita discusiones con la IA y contigo mismo.
4. **Notas por sesión.** Qué se hizo, cómo probarlo y qué falta. Con IA, sin esto cada sesión empieza de cero (en el kit: `plan/ESTADO.md`).
5. **Congela funcionalidades mientras la revisión está abierta.** Solo arreglos.
6. **Guarda el material de tienda en el repo**: textos de la ficha con recuento de caracteres, respuestas a cuestionarios y cada rechazo con su respuesta. La siguiente versión es copiar y ajustar.

## Técnica

7. **Documentación oficial antes que memoria.** Expo cambia cada pocos meses; lo que «sabe» la IA puede estar desfasado. Lee la doc de la versión del SDK del proyecto.
8. **`npx expo install` siempre**, y `expo-doctor` limpio. Las versiones las fija el SDK.
9. **Development build pronto.** Expo Go basta al principio; en cuanto entra algo nativo (mapas, login con Google, push, widgets), los problemas aparecen ahí. Mejor descubrirlos en el hito 2 que la semana del lanzamiento.
10. **Prueba el build de producción**, no solo desarrollo. Hay fallos que solo salen ahí: variables que no llegan al build en la nube, módulos que se cargan tarde.
11. **La configuración que falta debe gritar.** Una app sin sus variables sale vacía sin avisar; mejor un error con el nombre de la variable.
12. **Lógica separada de la interfaz y con tests.** Cuando una app pasó de un solo móvil a online, solo cambió de dónde salían los datos.
13. **Datos antes que código.** Lo que puede vivir en la base de datos llega a todos al instante, sin revisión.
14. **EAS Update para JS, versión nueva para lo nativo.** Permisos, plugins y librerías nativas obligan a pasar por la tienda.
15. **No pelees con Xcode.** Si una versión de Xcode no compila el SDK, compila en la nube de EAS antes que parchear Expo.

## Seguridad y privacidad

16. **Ninguna clave secreta en la app.** Todo lo `EXPO_PUBLIC_` lo puede leer cualquiera.
17. **RLS en todas las tablas desde la primera migración.** En una restauración, una de nuestras bases de datos quedó con permisos de borrado para la clave pública en tablas sin RLS: cualquiera con la app podría haberla vaciado.
18. **Los plugins añaden permisos que no usas** (micrófono, audio en segundo plano). Un permiso injustificado es un rechazo; un texto de permiso que falta, también.
19. **Una sola historia de privacidad**: la política, las etiquetas de la tienda, los textos de permisos y el marketing deben decir lo mismo. Declarar de más no penaliza; de menos, sí.
20. **Borrar la cuenta desde la app** es obligatorio si hay cuentas.

## Tiendas

21. **Abre las cuentas pronto**: Apple (99 €/año) y Google (25 $) tardan días en verificarse.
22. **Google, cuenta personal nueva**: 12 testers durante 14 días seguidos antes de producción. Recluta 15-20 y empieza en cuanto la ficha esté completa.
23. **Declara tu estado de comerciante en la UE (DSA).** Una app aprobada no aparecía en las tiendas europeas por no hacerlo.
24. **El nombre y el identificador son para siempre.** Comprueba que el nombre está libre antes de diseñar la marca.
25. **El backend gratuito se duerme.** Un revisor encontró la app rota por eso. Mantenlo despierto mientras la app esté publicada.
26. **Implementa y luego anúncialo.** Nada de superlativos, «oficial» o «IA» que no existan. Si usas información pública, deja claro que la app es independiente.
27. **Responder a un rechazo**: inglés, corto, verificable, sin hablar de lo que la app era antes. Tu respuesta puede provocar el siguiente rechazo.
28. **Publicación manual**: deja margen entre la aprobación y la salida.
29. **El revisor no es de tu ciudad**: dale una cuenta demo, instrucciones y lo que necesite para probar fuera de tu zona.

## Después del lanzamiento

30. **Conseguir descargas es lo fácil; que vuelvan es lo difícil.** Mide desde el día uno si los usuarios repiten y hacen la acción principal.
31. **El permiso de notificaciones, en el momento justo.** Pedido al abrir la app, casi nadie lo acepta, y luego no hay forma de recuperar a quien se va.
32. **Vídeo corto y constancia** fue lo que más trajo usuarios; las palabras exactas de los primeros usuarios son los mejores ganchos.
