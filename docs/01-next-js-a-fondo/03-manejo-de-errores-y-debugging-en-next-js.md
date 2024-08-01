# Manejo de errores y debugging en Next.js

Next.js ofrece un buen soporte de errores durante el proceso de desarrollo. Podemos utilizar la consola del navegador para detectar errores de cliente, pero también nos mostrará un modal cuando haya errores de servidor. Fuera de este sistema de errores, orientado a facilitar la experiencia de desarrollo, existen al menos otros dos tipos de error que nos va a interesar gestionar.

Gestionar errores es interesante porque no solo nos permite establecer cierta trazabilidad, sino que además evitará que nuestra aplicación se rompa y que podamos seguir utilizando sus secciones no comprometidas.

## Cómo gestionar errores de servidor

Next.js nos permite hacer una gestión de errores similar en la que haríamos en cualquier otro proyecto de Node.js en servidor. Esta gestión de errores deberá implementarse en los archivos `route.ts`, y podemos utilizar el segundo argumento del objeto `NextResponse` para insertar un código de error:

```javascript
export async function GET(request: Request) {
  return NextResponse.json({ error: "Internal Server Error" }, { status: 500 });
}
```

Además, Next.js nos ofrece la posibilidad de personalizar las páginas que queremos mostrar en caso de que el servidor nos devuelva un error `404`, mediante la convención `not-found.tsx`.

## Cómo gestionar errores de cliente

En la parte de cliente vamos a utilizar React.js, por lo que la gestión de errores no presenta particularidades respecto al uso cotidiano de esta librería.

## error.tsx

La convención `error.tsx` nos permite implementar una interfaz de usuario para el caso de errores. Ten en cuenta que se trata de componentes de cliente, por lo que deben incluir la directiva `'use client'`. Estos componentes recibirán dos parámetros por defecto:

1. **error**: contine el error capturado
1. **reset**: es una función que nos permite intentar recuperarnos del error volviendo a renderizar el segmento

Error crea un [React Error Boundary](https://react.dev/reference/react/Component#catching-rendering-errors-with-an-error-boundary), que nos permite aislar el error para que el resto de la interfaz siga funcionando y la parte perjudicada muestre un contenido alternativo.

Cualquier error arrojado en nuestra aplicación escalará hasta el `Error Boundary` más cercano. Debemos tener en cuenta, sin embargo, que **los elementos `layout.tsx` están contenidos dentro del `Error Boundary`**, por lo que si el error se produce en ellos, este seguirá escalando hasta el nivel superior. Lo mismo ocurre con los archivos `template.tsx`. ¿Cómo capturamos entonces los errores producidos en el root layout? Para ello disponemos de la convención `global-error.tsx`. Dado que este archivo envuelve el resto de la aplicación, deberá incluir las etiquetas `<html>` y `<body>`.

## El modo verboso

Si las herramientas presentadas no son suficientes para nuestro proceso de depuración, debemos recordar que podemos recurrir a cualquier otra herramienta de depuración. Solo tendremos que considerar si estamos depurando componentes de cliente o de servidor. Además, disponemos de la flag `--debug`, que podemos proporcionar al comando `npm run build --debug`, para obtener más información sobre el proceso.
