# Características avanzadas y nuevas actualizaciones en Next 15

Next.js es uno de los frameworks más utilizados en la actualidad y, como tal, está en constante evolución. Algunas de sus actualizaciones ponen a nuestra disposición numerosas herramientas, nuevas convenciones o nuevas formas de organizar el proyecto.

En estos momentos hay un **Release Candidate (RC)** para la versión 15 de Next.js. Las novedades que incorpora son:

- Soportará el compilador React 19 RC (React Compiler) y mejorará los errores de hidratación
- Se ofrecerá una opción de configuración en `layout.tsc` y `page.tsc` para potenciar la adopción del Partial Prerendering (**PPR**). El PPR va a facilitar que, en rutas donde estemos utilizando funciones dinámicas (`cookies()`, `headers()` y similares) podamos combinar un renderizado estático con uno dinámico. Ahora mismo, el uso de tales funciones convierte a la ruta en dinámica, lo que impide que disfrutemos de los beneficios a nivel de rendimiento que conllevan las rutas estáticas
- Implementará la API `next/after`, que nos permitirá ejecutar código cuango una respuesta se haya terminado de stremear
- Incorporará la opción de incluir `Turbopack` en el proceso de desarrollo local
- Nos permitirá añadir nuevas configuraciones a los Routers para mejorar la gestión de paquetes de terceros

## El caso de fetch

Tal vez el caso más destacado de actualización tiene que ver con la función `fetch`. Una de las decisiones más conflictivas del framework fue la de extender el comportamiento del fetch nativo del navegador, ofreciéndole las funciones de caché y revalidación. Desde la versión 15, Next no cacheará por defecto las peticiones fetch, los GET expuestos en `route.ts` ni la navegación del cliente.

Esto no impide que utilicemos la caché, ya que podemos:

- Utilizar `cache: 'force-cache` en nuestras peticiones
- Añadir `export const dynamic = 'force-static'` o `export const fetchCache = 'default-cache'`en nuestra ruta

Además, algunas rutas especiales seguirán siendo estáticas, como `sitemap.ts`, `opengraph-image.tsx` y otros archivos de metadatos.
