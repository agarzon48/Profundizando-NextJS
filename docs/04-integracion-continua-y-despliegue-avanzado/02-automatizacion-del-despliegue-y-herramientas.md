# Automatización del despliegue y herramientas

Existen diferentes estrategias para la automatización del despliegue, pero su enfoque termina reposando siempre en el mismo elemento: los hooks. Git dispone de hooks que se disparan en determinados momentos del ciclo de producción, siendo los principales aquellos anteriores o posteriores a la realización de un commit o un push. Además, cualquier herramienta de gestión de código (`github`, `azure`...) nos permite conectar funcionalidades a estos hooks.

Esto nos permite establecer una rama privilegiada que, cada vez que reciba una incorporación de código, avise a nuestro servicio de hosting para que pueda actualizar su código fuente lanzando un `pull`. Actualizado el código en nuestro servidor podemos lanzar procesos como `npm run build`, que construyan una nueva versión del proyecto para servir al público. Vercel automatiza todo este proceso reduciendo la configuración necesaria, siendo esta la herramienta que vamos a utilizar en el curso.

Si no hubiéramos optado por Vercel como servicio de despliegue tendríamos que configurar todo este proceso manualmente. Para ello podemos utilizar herramientas como `jenkins`.
