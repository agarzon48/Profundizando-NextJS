# Monitoreo, escalabilidad y optimización en producción

Vercel nos ofrece una serie de herramientas de monitorización, que analizaremos en este curso. Además, Next.js tiene soporte para herramientas de telemetría como `OpenTelemetry`. Si no optáramos por desplegar en Vercel sería conveniente que integremos herramientas de monitorización como [Grafana](https://grafana.com/).

Integrar servicios de monitorización nos va a permitir entender cómo está funcionando nuestra aplicación: cuántos usuarios estamos recibiendo, cómo interactúan con nuestra aplicación, cómo afecta esto al uso de memoria o espacio de nuestro servidor... También es importante que dispongamos de un sistema de registros que nos permita trackear los eventuales errores que se produzcan en la aplicación. A estas herramientas, más propias de backend, podemos añadir otras como `lighthouse`, que nos ofrecerá información importante respecto al renderizado y uso de la aplicación.

> En este curso vamos a analizar los servicios de monitorización básicos de Next.js, utilizando:
>
> - [Observability](https://vercel.com/products/observability), una suite de herramientas de analítica para entender el tráfico de nuestra aplicación
> - **Speed Insights**, que nos ofrece información pormenorizada acerca del rendimiento de nuestra aplicación
> - **Runtime logs**, que nos ofrece una colección de registros generados mediante `console.log` en las funciones ejecutadas en servidor y edge

## Optimizaciones en producción

Para mejorar la operatividad de nuestra aplicación, conviene que contemos con un **plan de respuesta**. Este debe incluir los canales de comunicación y escalado para recibir y gestionar feedback y notificaciones de errores, así como una estrategia de rollback que nos facilite la recuperación en caso de error. Como hemos visto, resultará fundamental contar con herramientas de monitorización.

Además, deberíamos asegurarnos de que estamos sirviendo adecuadamente nuestro contenido, lo que incluiría:

- Comprobar el funcionamiento de la caché, asegurándonos de que no lanzamos procesos de construcción innecesarios
- Utilizar algún servicio DNS que optimice el envío de datos y evite caídas en momentos de migración o actualización
- Asegurarnos de que la capacidad de nuestro hardware es adecuada a los requisitos de nuestros procesos
- Establecer tests automatizados sobre funciones críticas, seguridad de la aplicación y salud del código (podemos utilizar herramientas como [Conformance](https://vercel.com/docs/workflow-collaboration/conformance/getting-started))

A nivel de seguridad, deberíamos cumplir al menos los siguientes requisitos:

- Implementar una **Content Segurity Policy (CSP)** y cabeceras de seguridad adecuadas
- Proteger las ramas de nuestro repositorio y el acceso a nuestro servidor y a las pipelines de despliegue
- Configurar un **Web Application Firewall (WAF)** para monitorizar y, en su caso, bloquear el tráfic entrante
- Gestionar un sistema de registros y eliminar aquellos que ya estén desfasados para tener información sobre todos los procesos y acciones ejecutados en nuestra aplicación
- Utilizar servidores o dominios de previsualización para realizar las pruebas y comprobaciones requeridas sin necesidad de llevarlas a producción
- Utilizar _lockfiles_ para garantizar que las versiones de dependencias utilizadas son las mismas en todos los entornos
- Gestionar roles de usuarios restrictivos bajo una política de confianza-cero
- Lanzar tests de estrés para comprobar la resiliencia de nuestro sistema
