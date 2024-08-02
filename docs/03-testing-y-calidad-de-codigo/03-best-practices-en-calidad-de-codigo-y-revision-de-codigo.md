# Best practices en calidad de código y revisión de código

## Mejores prácticas en testing

Aunque este no sea un curso de testing, comparto algunos consejos a la hora de implementar los tests:

1. Generalmente se recomienda que haya tests unitarios que cubran todos los bloques de nuestra aplicación. Los tests de integración deben comprobar solo el funcionamiento coordinado de tales bloques, por lo que serán menos numerosos. Por su parte, los tests E2E suelen reservarse a los elementos más importantes de la aplicación, pero en el caso de Next.js y dada la relativa novedad de los componentes asíncronos, es posible que existan más tests E2E que en otro tipo de proyecto
1. Conviene que al testear sigamos el patrón **AAA**: Arrange, Act, Assert
1. Debemos evitar los efectos secundarios en los tests. Para ello podemos utilizar herramientas como los mocks y los spies
1. No debemos testear código sobre el que no tengamos potestad. Esto incluye librerías de terceros y APIs
1. Si no lo has hecho nunca, te recomiendo que pruebes el TDD: aunque requiere disciplina, induce a un código más sencillo y facilita que los tests cubran el 100% de nuestra aplicación. Además es una forma de asegurarte de que el test que estás escribiendo es pertinente

## Mejores prácticas en calidad de código

1. Utiliza un linter para mantener un código óptimo y más mantenible
1. Utiliza herramientas como `prettier` para garantizar la homogeneidad de tu código
1. Si el proyecto lo permite, utiliza TypeScript para garantizar la solidez de tus tipos
1. Implementa tests que cubran gran parte de tu aplicación, sin obsesionarte por el coverage
1. Apuesta por el código sencillo y evita la sobreingeniería y las microoptimizaciones

## Mejores prácticas en revisión de código

1. Revisa cuidadosamente tu código antes de mergearlo con la fuente o pasarlo a revisión del equipo
1. Protege las ramas de tu repositorio para que no se pueda incorporar código que no haya pasado por un proceso de revisión
1. Incorpora pipelines y hooks para lanzar procesos automatizados de lint, tsc, build y test antes de incorporar el código a las ramas principales
1. Dedica el tiempo necesario a revisar las aportaciones de tus compañeros y compañeras
1. Realiza aportaciones del mínimo tamaño posible para evitar que se cuelen errores en el proceso de revisión
