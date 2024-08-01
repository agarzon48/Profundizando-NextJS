# Estrategias de renderizado y optimización del rendimiento

Next.js pone a nuestra disposición tres estrategias de renderizado:

1. Static
1. Dynamic
1. Streaming

### Static Rendering (default)

El renderizado por defecto es estático. Esto significa que, durante el build time (es decir, cuando lancemos `npm run build` o sus análogos con otros gestores de paquetes) nuestro servidor renderizará todos nuestros componentes de React y generará archivos estáticos HTML.

Algunos datos son dinámicos. Por ejemplo, en nuestro proyecto vamos a generar un sistema de ticketing, y los tickets van a variar con el tiempo. ¿Significa esto que tengo que lanzar `npm run build` para actualizar mi aplicación con los últimos tickets?

No. Next.js incluye un sistema de revalidación (**data revalidation**) que nos permite eliminar la información desfasada y actualizarla cada cierto tiempo o cada vez que el usuario visita la página. Entraremos en más detalle con ejemplos en nuestra app.

#### Ventajas del static rendering

- Nuestra aplicación se crea en build time, por lo que la carga de trabajo del servidor (y sus tiempos de respuesta) son inferiores
- Si no necesitamos una API, podemos utilizar un servidor estático, que suele ser más económico que un server que soporte Node.js

#### Desventajas del static rendering

- No tendremos acceso a ciertas funcionalidades dinámicas. Por ejemplo, nuestras páginas no pueden reaccionar a las cookies de la petición, ya que servimos el mismo archivo estático en todos los casos

### Dynamic Rendering

En ocasiones no podemos pre-renderizar un archivo estático porque cada usuario debe recibir una versión diferente de la página o aplicación. Por ejemplo, si utilizamos un sistema de roles es posible que el administrador deba disponer de más funcionalidades que el usuario básico. O cuando buscamos en un e-commerce como Amazon donde se incluyen filtros y preferencias en la URL, estamos visitando cada vez una dirección diferente, por lo que no podremos pre-generar todas las opciones disponibles.

En los casos en que optemos por utilizar rutas dinámicas, Next.js renderizará dinámicamente la ruta cada vez que esta sea visitada. Esta opción, normalmente, se tomará por Next.js automáticamente para optimizar nuestro código, pero es importante que sepamos que no podemos depender del renderizado estático cuando:

1. No estemos guardando información en caché, lo que requiere que se vuelva a crear la página antes de cada visita
1. Utilizamos información de las cabeceras o cookies de la petición para configurar nuestra página
1. Utilizamos los query params para personalizar la página

#### Ventajas del dynamic rendering

- Podemos acceder a información relevante en las cabeceras o cookies de las peticiones

#### Desventajas del dynamic rendering

- No disponemos de caché, por lo que generar cada página será más costoso que si fuera estática

### Streaming

Siendo un concepto más avanzado, lo trataremos en detalle en el curso "Profundizando en Next.js". Sin embargo, quiero hacer una introducción para que conozcas las tres estrategias de renderizado de Next.js.

El streaming nos permite renderizar progresivamente nuestra UI desde el servidor.No es necesario que hagamos ninguna configuración compleja, ya que disponemos de la convención `loading.tsx` y del `<Suspense />` de React para indicar a Next.js qué queremos que se visualice en la página mientras nos llegan todos los fragmentos de la UI.

## ¿Cuándo quiero utilizar CSR y cuándo SSR?

Optar entre CSR y SSR dependerá de cada funcionalidad que se quiera incorporar a cada proyecto. Cualquier proyecto de Next.js combina ambas estrategias.

Como guías generales:

### Quieres utilizar SSR cuando...

- Tienes que obtener datos (fetch). Optimizarás tiempos de carga y podrás cachear respuestas
- Tienes que proteger información. Los tokens y claves API se quedarán en el servidor
- Quieres agilizar la carga de la página. Podrás reducir la cantidad de JS que debe enviarse al cliente y ejecutarse en este

Si puedes optar entre SSR y CSR, lo más frecuente es que debas utilizar SSR. Normalmente utilizamos CSR cuando SSR nos limita (por ejemplo, necesitamos escuchar un click del usuario).

### Quieres utilizar CSR cuando...

- Necesitas interacción del usuario. Solo en el cliente se pueden lanzar eventos como un click o el cambio de un input.
- Necesitas utilizar hooks que dependan del ciclo de vida del componente. useState, useEffect y otros hooks se lanzan en momentos determinados del ciclo de vida del componente, por lo que dependen del DOM y por tanto solo pueden lanzarse en el cliente. Esto incluye los **custom hook** que dependan de otros hooks basados en el ciclo de vida del componente
- Necesitas utilizar APIs del navegador. Los componentes SSR no pueden acceder a estas APIs.
- Necesitas utilizar **class components**. Cosa que solo debería ocurrir en proyectos _legacy_.

## Optimización del rendimiento en Next.js

El SSR nos permite utilizar la potencia del servidor, que por regla general será superior a la del dispositivo del usuario, además de otras ventajas como la caché, el precompilado o el **Incremental Static Regeneration (ISR)**. Esto implica que, siempre que podamos permitírnoslo, deberíamos trabajar con componentes del servidor y aprovechar el build time para acelerar el trabajo de renderizado.

Si no necesitamos utilizar funcionalidades de servidor, Next.js nos permite crear **páginas estáticas**, que podríamos servir desde cualquier servidor estático (lo que probablemente optimice también nuestros costes) y distribuir desde una CDN para mejor rendimiento. Esta estrategia va a ser siempre la más óptima, ya que el servidor se limita a enviar un archivo sin tener que procesar ningún tipo de información.

En ocasiones, las páginas estáticas no serán suficientes porque necesitaremos realizar actualizaciones puntuales. Por ejemplo, si desarrollamos un blog, una web corporativa o si nuestros servicios reposan sobre el cliente (utilizando Firebase, Google Maps u otras APIs similares). Para estos casos podemos aprovechar la estrategia ISR. Esta técnica nos permite construir nuestro sitio web y revalidar los datos cada cierto tiempo.

Al utilizar ISR, Next.js genera una página y guarda sus datos dinámicos en caché. Durante los accesos posteriores se servirá esta versión de la página, por lo que el tiempo de carga será análogo al que tendríamos en caso de utilizar archivos estáticos. Sin embargo, tras expirar el proceso de revalidación, la primera visita lanzará un proceso de purga de la caché y revalidación. Esta opción nos permite incluso visitas a páginas que no existían en build time, cuyo funcionamiento será similar: durante la primera consulta se creará la versión estática en caché y será esta la que se envíe a las siguientes peticiones hasta que la caché quede invalidada de nuevo.

**Ten en cuenta que si optas por una estrategia ISR deberás gestionar la caché por defecto de tu servidor o CDN**. En caso contrario podrían invalidar las revalidaciones configuradas.

La revalidación se puede hacer también a demanda. Una estrategia común es establecer un endpoint aprovechando los archivos `route.ts` desde la cual lanzar un `res.revalidate('/path')`. Esta estrategia es compatible con la revalidación por tiempo.

### Otras estrategias de optimización

- Recuerda utilizar los componentes `Image` y `Scripts` para optimizar tus aplicaciones de Next.js
- Además de estos componentes disponemos de `Link`, que no solo facilita la navegación sino que además lanzará un pre-fetch del recurso vinculado para que el usuario pueda acceder al mismo lo más rápido posible
- Optimiza los recursos estáticos antes de subirlos a tu proyecto, ya que no siempre se pueden o deben cachear
- Utliza el sistema de fuentes de Next.js (`next/font`) para incrementar la eficiencia y privacidad de tus tipografías
- Aprovecha el lazy loading (`next/dynamic` y `React.lazy`) para facilitar la interacción del usuario con tu página lo antes posible. Combínalas con los archivos `loading.tsx` para aprovechar el **streaming** nativo de Next.js
- Estudia el rendimiento de tu aplicación con `@next/bundle-analyzer` y su rendimiento con `useReportWebVitals` o `Lighthouse`
