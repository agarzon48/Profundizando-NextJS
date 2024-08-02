# Introducción al testing en Next.js

El testing no es más que la comprobación de que nuestro código hace aquello para lo que lo hemos escrito. Lo hacemos constantemente mientras desarrollamos, cada vez que consultamos en la consola o en el navegador si estamos renderizando lo que necesitamos. Sin embargo, cuando hablamos de testing solemos hacer referencia en realidad al testing automatizado.

El testing automatizado nos permite describir los comportamientos esperados de nuestra aplicación, utilizando código. Posteriormente podemos utilizar un test runner para ejecutar este código y obtener un feedback relativamente intensivo acerca de cómo se está comportando nuestra aplicación. Esto ofrece importantes ventajas respecto al testing manual:

1. Es más exhaustivo, ya que si hacemos tests manuales debemos comprobar todos los casos cada vez que introducimos un cambio, y podemos olvidarnos de comprobar determinados escenarios o partes de nuestra aplicación
1. Es más rápido, ya que el test se ejecuta automáticamente y no tenemos que repetir el mismo flujo de uso una y otra vez
1. Podemos integrarlo en un proceso de desarrollo, lo que permite que no implementemos o despleguemos código si ha roto prestaciones de la aplicación previamente implementadas (hablaremos más sobre esto al abordar el CI/CD)

Esto no es un curso de testing, sino de testing en Next.js, así que no dedicaré mucho más tiempo a las virtudes de implementar tests o sus rudimentos. Entremos directamente a la implementación del test.

## Tests unitarios

Los tests unitarios suponen revisar bloques o unidades de código aisladas. Por ejemplo, en una aplicación de Next.js podríamos aplicarlos a hooks, componentes o utilidades. Algunas herramientas útiles para realizar este tipo de tests con `jest` o `vitest`. Ten en cuenta que, como vamos a testear componentes de cliente, necesitaremos emular un navegador y la interacción del usuario con nuestros componentes para saber cómo se comportan.

## Tests de integración

Los tests de integración comprueban cómo funcionan varias unidades juntas. Por ejemplo, una sección de la aplicación o una página. Para realizar este tipo de tests podemos utilizar las mismas herramientas que para hacer tests unitarios, ya que la lógica viene a ser la misma. De hecho es frecuente que exista debate sobre dónde está la frontera entre el test unitario y el de integración.

## Tests de snapshot

Permiten trackear cambios en una instantánea tomada sobre el código renderizado. Si el código renderizado cambia, la captura también, y al compararla con la establecida como correcta saltarán las alarmas. Estos tests y los tests visuales son la herramienta idónea para detectar cambios inesperados en nuestra interfaz. Tanto `jest` como `vitest` nos permiten implementar este tipo de tests.

## End to end (E2E) testing

Los tests E2E simulan la interacción del usuario con la aplicación. Esto implica un mayor coste de planteamiento y ejecución, por lo que normalmente serán menos numerosos que los test de integración, que a su vez serán menos numerosos que los unitarios. Este tipo de tests requiere otras herramientas, como `cypress` o `playwright`.

## Particularidades del testing en Next.js

- En Next.js vamos a utilizar librerías de cliente. De modo que tendremos que hacer tests en frontend que emulen interacciones del usuario, por lo que es oportuno apoyarse en herramientas como [testing-library](https://testing-library.com/docs/react-testing-library/intro/)
- Los componentes asíncronos son relativamente nuevos en el ecosistema, así que en estos momentos es más sencillo utilizar E2E que tests unitarios
