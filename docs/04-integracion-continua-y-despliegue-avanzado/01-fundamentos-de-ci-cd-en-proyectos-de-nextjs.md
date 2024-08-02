# Fundamentos de CI/CD en proyectos de Next.js

CI/CD son las siglas de Continuous Delivery/Continuous Integration. Se trata de una filosofía que plantea:

1. El software se debe producir en ciclos cortos
1. Estos ciclos deben derivar en un producto que pueda liberarse de forma segura y confiable lo más rápido posible

El resultado de asumir este enfoque es la producción y despliegue constante de funcionalidades, alejándose de un planteamiento basado en grandes versiones. Al ser constante, el proceso nos permite liberar y testear pequeñas funcionalidades y construir las más amplias de forma incremental. Un error en el despliegue se corrige con un rollback a la versión anterior, que al ser relativamente breve permite una corrección rápida.

Para garantizar la seguridad y confiabilidad de los cambios, este enfoque requiere la implementación de tests automatizados. La integración de pipelines o hooks que disparen este proceso automáticamente minimiza los errores que pueden llegar a la rama principal, que no tiene por qué ser la de producción. Sin embargo, las posiciones más radicales del CI/CD abogan por un flujo de trabajo que elimina las ramas secundarias, ya que estas facilitan un ciclo de producción más amplio y ofuscan el acceso al código que están escribiendo el resto de compañeros y compañeras.

> Como en este proyecto vamos a utilizar Vercel, configuraremos allí nuestro flujo de trabajo. Vercel está optimizado para integrar un proceso basado en CI/CD, pero podríamos integrarlo también en otros entornos utilizando hooks o herramientas como `jenkins`.
