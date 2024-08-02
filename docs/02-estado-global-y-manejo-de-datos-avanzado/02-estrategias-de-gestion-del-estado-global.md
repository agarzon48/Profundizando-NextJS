# Estrategias de gestión del estado global

Si tienes experiencia trabajando con React.js sabrás que la gestión del estado puede complicarse en el momento en que la aplicación crece. Precisamente por ello existen herramientas orientadas a la gestión del estado global como [Redux](https://react-redux.js.org/), [Recoil](https://recoiljs.org/) o [Zustand](https://zustand-demo.pmnd.rs/).

## Prop drilling y forward props

React, de forma nativa, nos conduce a comunicar componantes padres con sus componentes hijos vía props. No hay un concepto análogo para que el componente hijo se comunique con el componente padre, pero lo cierto es que les podemos pasar una función que conlleve la actualización del estado como prop. Esto permite que un evento en el hijo dispare una actualización de estado en el padre, lo que en la práctica supone una comunicación del hijo al padre que conocemos como _forward props_.

Aquí ya podemos observar un primer sesgo de React: está diseñado para que los componentes padres se comuniquen con los hijos. Y, aunque podemos hacer una comunicación bidireccional, requeriremos de un código más complejo. Además, percibimos otro problema potencial: cuando tengo que pasar props de padres a hijos, si el componente que necesita utilizar la información está muy anidado, los componentes intermedios tendrán que transmitir la prop sin llegar a utilizarla. Conocemos como _prop drilling_ al fenómeno que se da cuando nuestros componentes tienen un gran número de props, utilizando tan solo una pequeña fracción de las mismas y utilizando el resto solo para pasarlas a sus descendientes.

El prop drilling termina derivando en un código extenso, difícil de leer y mantener. Y otro tanto de lo mismo ocurre con las forward props, por lo que las herramientas nativas de React no eran adecuadas para gestionar estados cuando hay un elevado nivel de anidación. Cosa que pasa, por ejemplo, en los estados globales, ya que deben compartirse con toda la aplicación. No tenía herramientas adecuadas... hasta que las implementó.

## Flux

Consciente de estos problemas, el equipo de Meta (padres de React.js) empezó a utilizar una arquitectura que denominaron `Flux`. La idea era relativamente sencilla: mantener un flujo de estado unidireccional.

Dado que React premia el traspaso de estados de padres a hijos, Flux pone el énfasis en un nivel elevado de nuestro árbol de componentes, que es el que maneja el estado de todos sus descendientes. Esto nos permite centralizar el estado y elimina problemas de sincronización entre los estados de varios componentes, pero todavía no resuelve el problema del prop drilling.

## Contexto

Consciente de que necesitaba una herramienta para resolver esta problemática, el equipo de React incorporó el concepto de contexto. El contexto se compone de dos piezas:

1. En primer lugar tenemos el propio contexto, que es un componente que almacena el estado global de nuestra aplicación o una sección de la misma
1. En segundo lugar tenemos el provider, que es un componente que permite a todos sus hijos acceder al estado global

La propuesta del contexto resuelve los problemas de las forward props (el contexto es el que gestiona el estado, y simplemente expone funciones que permiten a los hijos manipularlos: es unidireccional) y del prop drilling (las props ya no tienen que pasar de padre a hijo: basta con que un componente esté dentro del provider paara que pueda acceder a su estado).

## Librerías de gestión de estado

El contexto de React (actualmente utilizado mediante la función `createContext` y el hook `useContext`) tardó en llegar. Y mientras se hacía oficial aparecieron librerías como `Redux` que nos ofrecían soluciones de conveniencia para cubrir sus necesidades. Además, tiene un problema: al contener el estado que afecta a numerosos componentes, fuerza el re-renderizado de todos ellos cada vez que el estado cambia.

Esto no es problemático para estados que cambian de forma relativamente infrecuente (sesiones, idiomas, tema oscuro...), pero supone una sobrecarga si lo queremos utilizar en componentes con un mayor ratio de actualización (por ejemplo, un text input).

Resolver este problema es relativamente sencillo: basta con utilizar un patrón subscriber para que solo se actualicen los componentes afectados por cada cambio de estado específico. Pero aunque sea sencillo requiere configuración, por lo que no es de extrañar que las librerías que fueron apareciendo resolvieran estos problemas. En este sentido, `Redux` ganó bastante popularidad, siendo utilizado todavía a día de hoy en muchas aplicaciones en producción. Y con el tiempo han ido apareciendo alternativas como `Recoil` o `Zustand`.

## La gestión de estado en las ramas de Next.js

Next.js elimina parte de la necesidad de contextos gracias a su sistema de caché. Y ello porque no hace falta que guardemos en un estado la respuesta de una API, ya que se va a cachear y podemos repetir la llamada tantas veces como queramos, sin que la llamada se realice realmente dado que se nutrirá con los datos de la caché (siempre que esta esté activada).

Sin embargo, hay muchos casos en los que todavía tenemos que gestionar estados compartidos, y aquí aparece otro reto: Si varios usuarios están utilizando el mismo servidor, ¿cómo garantizamos que cada uno tiene su propio estado?

## La gestión de estado global

Para mantener el estado de cada cliente aislado podemos utilizar componentes de cliente apoyados en un contexto o bien en una librería de gestión de estado. Otras opciones que nos permiten persistir el estado son:

1. Utilizar sistemas de persistencia, como las APIs de navegador `localStorage` o `indexedDB`. El problema es que este estado no se puede compartir entre dispositivos y tendremos que asegurarnos de sincronizar adecuadamente los estados de cada uno de nuestros componentes con el sistema seleccionado
1. Optar por soluciones de backend, como la base de datos. En este caso persistimos un estado que se aplicará a diferentes dispositivos, pero también generamos una sobrecarga en la infraestructura, ya que cada petición del cliente tendría que pasar previamente por la consulta a la DB de sesiones
   1. Ten en cuenta que el backend tiene acceso a otras estrategias de gestión de estado, como son las cabeceras y cookies de la petición o la URL, donde podemos utilizar fragmentos dinámicos y query params
1. Utilizar sistemas de tokens o similares, como `jwt`, que nos permiten almacenar cierta información del usuario.

> En este curso vamos a implementar el gestor de estados Zustand e incluiremos mecanismos de persistencia. Sin embargo, la filosofía de todos los gestores globales de estado viene a ser la misma: aislar el estado global en un punto alto del árbol de componentes y que todos los descendientes accedan vía hook
