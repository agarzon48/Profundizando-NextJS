# Integración avanzada de APIs y manejo de datos

Quitando el caso de librerías como `Firebase`, que nos permiten gestionar APIs desde el cliente, Next.js nos ofrece diferentes estrategias para integrarnos con una API:

- **Server Components**: los componentes de Next.js, por defecto, son componentes de servidor. Esto significa que podemos pre-renderizarlos en el server sin riesgo de exponer tokens de acceso o claves de API
- **`route.ts`**: la convención `route.ts` nos permite escribir una API sin esfuerzo en nuestro proyecto de Next.js. Podemos exponer endpoints que podrán consumir tanto nuestros componentes de servidor como nuestros componentes de cliente, si bien está orientada a los segundos principalmente
- **Server Actions**: una relativa novedad de Next.js son las server actions. Se trata de funciones asíncronas que se ejecutan en el servidor y se pueden utilizar tanto en componentes de cliente como de servidor para gestionar envíos de formularios y mutaciones de datos

## ¿Cuándo utilizar Server Components?

Si nuestra API nos va a ofrecer información que debemos renderizar en pantalla o que afecta a la interfaz renderizada, podemos hacer las peticiones necesarias directamente desde nuestros Server Components, ya que son asíncronos. Esta es probablemente la aproximación más sencilla, ya que basta con hacer un fetch y utilizar la respuesta para poblar o configurar el componente.

Aunque al iniciarse en Next.js es normal echar mano de los archivos `route.ts` para integrar APIs, lo cierto es que en la mayoría de ocasiones no es necesario que dupliquemos la estructura de la misma con nuestra propia API.

## ¿Cuándo crear nuestra propia API?

En ocasiones estaremos forzados a utilizar la API integrada directamente desde los componentes de cliente. Por ejemplo, cuando necesitemos registrar eventos.

Para estas situaciones, el enfoque tradicional consiste en crear una API aprovechando los archivos `route.ts`. Este enfoque no tiene mayor complejidad, ya que basta con configurar cada uno de los endpoints que necesitemos exponer. Como ventaja, nos ofrecerá la posibilidad de conectarnos a estos endpoints a través de otros aplicativos, por lo que será nuestra única opción cuando queramos interactuar con nuestros servicios no solo desde nuestra aplicación sino también desde fuentes externas.

## ¿Cuándo utilizar Server Actions?

Las server actions son funciones asíncronas que se ejecutan en el servidor. Esto implica que podemos acceder a todas las APIs y servicios del servidor y que no corremos el riesgo de exponer información sensible. Como pasa con los Server Components, las server actions se van a ejecutar desde nuestra aplicación, sin permitir la integración de aplicativos de terceros. Sin embargo, se pueden lanzar tanto desde componentes SSR como desde componentes CSR.

Aunque el uso estándar de una Server Action es en un formulario (se pueden disparar desde el atributo action del elemento `<form>`, de ahí su nombre), también pueden dispararse desde otros elementos, librerías de terceros o hooks como `useEffect`. Se pueden ejecutar tras terminar el proceso de hidratación de la página, y utilizan el método `POST`.

Al poder reutilizarse, son una buena alternativa a la construcción de una API y reducen la estructura del proyecto. Sus únicas limitaciones reseñables son:

- Deben incluir la directiva `'use server'`
- Sus argumentos deben ser serializables por React (lo que excluye elementos JSX, otras funciones, clases y símbolos)

Una ventaja destacable de las Server Actions en Next.js es que integran la arquitectura de caché y revalidación. Esto implica que al invocarlas podemos recibir la actualización de nuestra UI y la información actualizada en la misma petición (mediante el método `revalidatePath`, importado desde `next/cache`). Además, al poder conectarlas a formularios podemos evitar la engorrosa gestión de estado derivada de React, pues una Server Action asignada al atributo action de un formulario recibe como parámetro el estado del formulario. Además de este parámetro, podremos asignare otros adicionales utilizando el método `bind()` o bien mediante la técnica tradicional de incluir campos ocultos.

También cabe destacar la posibilidad de utilizar el hook de React `useFormStatus`, que nos permite determinar si el formulario se está enviando. Gracias a esta información podemos, por ejemplo, renderizar estados de carga. Otro hook útil es `useOptimistic`, que nos permitirá renderizar el nuevo estado sin necesidad de esperar a la respuesta del servidor.

Las Server Actions pueden acceder a funciones de Next.js que solo se pueden ejecutar en el servidor, como `redirect`, `cookies` o `headers`.

### Precaución: protege tus Server Actions

No olvides que una Server Action no deja de representar la exposición de una API pública. De modo que cuando estés trabajando con recursos protegidos deberías añadirles una capa de autenticación para evitar que usuarios no autorizados accedan a los mismos.
