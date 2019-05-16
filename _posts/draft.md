Por favor, no pervirtáis la tecnología.
Muchas, muchas veces me he encontrado y me encontraré proyectos en los que se utiliza una herramienta o conjunto de herramientas diseñadas con un propósito muy concreto. Esto es bueno, es [filosofía UNIX][unix_philosophy]: haz una cosa, pero hazla bien. Literalmente, de la Wikipedia: 
The UNIX philosophy is documented by [Doug McIlroy][douglas_mcilroy] in the Bell System Technical Journal from 1978:

Make each program do one thing well. To do a new job, build afresh rather than complicate old programs by adding new "features".
Para flujos complejos utiliza varias de estas herramientas que hacen cosas muy bien, encadenadas.
De hecho, el paradigma de los microservicios, revolución que estamos viviendo estos últimos años en el desarrollo del software, va muy de la mano de esta idea que data de 1978 y es que ya me lo dijeron mis padres hace tiempo: no hay nada nuevo bajo el Sol.

Un ejemplo maravilloso es "cat", simplificando un poco su función, este comando saca por consola (stdout) el contenido de un archivo con un conjunto reducido de opciones para elegir qué hacer con caracteres especiales como tabulaciones, retornos de carro, etc. Hace tan bien su función que a veces nos ocurre que lo usamos hasta sin hacer falta (con el típico ```cat archivo | grep loquesea```, cuando podría hacerse con ```grep archivo loquesea```. Que tire la primera piedra quien no lo haya hecho alguna vez :p ).

Hasta aquí todo bien. El problema surge cuando se quiere añadir una funcionalidad que no cubren las herramientas del stack actual y no se quiere modificar ni el stack tecnológico ni la forma de trabajar. Se busca una forma de forzar las herramientas a hacer algo para lo que no están diseñadas.

Me gustaría ilustrar esto que digo con un par de ejemplos:

**Ejemplo 1**: (y barreré un poco para casa con el ejemplo enfocándolo en mi área de especialización: k8s, GKE, contenedores...):
En una compañía donde se realizaba un filtrado de IPs para entender a qué aplicación correspondía el tráfico en función del pool de IPs de procedencia, se estaba implantando Kubernetes.
Kubernetes es una herramienta de orquestación de contenedores, nos abstrae de una serie de cosas que ocurren "por debajo" para que nos preocupemos de otras más ligadas a nuestra aplicación. Mantiene un número de copias constante de nuestras aplicaciones o escala dicho número en función de la carga, distribuye nuestras aplicaciones a lo largo del cluster para garantizar alta disponibilidad, se encarga de descubrir dónde es desplegada la aplicación y si está preparada para recibir peticiones para posibilitar que el resto de aplicaciones se puedan comunicar con ella... y una larga lista de funcionalidades acorde a lo que podemos esperar de un orquestador de contenedores.
Una de las cosas de las que nos abstrae, precisamente, es de la gestión de IPs de los contenedores desplegados. Los contenedores nacen y mueren, se mueven entre nodos, escalan y desescalan... podría decirse que es un recurso efímero, con un ciclo de vida normalmente más corto que el de una máquina virtual y por ende, sus IPs también son un recurso efímero.
Se puede elegir sobre qué grupo de nodos del cluster caen tus contenedores, para por ejemplo tener grupos de gran capacidad de cómputo para tus aplicaciones más exigentes. Además sobre GKE y a día de hoy, cada nodo tiene asignado su rango de IPs para los contenedores, por lo que cualquier trabajador voluntarioso y proactivo podría verse tentado de usar estos rangos de IPs asignados a los nodos, configurando el despliegue de la aplicación en nodos concretos para así poder filtrar el tráfico en función de las IPs y ver a qué aplicación se corresponde qué tráfico. Esto, sin embargo, sería mala idea. Rebuscando un poco en la documentación se puede ver cómo se [desaconseja este uso][gke_networking_doc]: 
"Therefore, a Pod's IP address is an implementation detail, and you should not rely on them. "
GKE no ha sido diseñado pensando en el uso de las IPs de los contenedores, es algo de lo que nos abstrae. Los nodos del cluster aparecerán y desaparecerán, cambiando los rangos de IPs asignados a estos y lo que es peor, nadie nos asegura, al no ser un objetivo de GKE como herramienta, que el comportamiento de la asignación de IPs sea el mismo en futuras actualizaciones! Es decir, que incluso después de haber hecho el esfuerzo de conseguir ~~pervertir~~ adaptar la herramienta a nuestras necesidades, en la próxima actualización podría funcionar de forma diferente tirando por tierra nuestros esfuerzos. Esto es, por lo tanto, un claro ejemplo de perversión de una tecnología.

**Ejemplo 2**: Aquí me gustaría poner un ejemplo relacionado con el mundo de desarrollo de software, en plan usar librerías que no han sido diseñadas para una cosa, para eso o algo del estilo.

**Ejemplo 3**:
Clodoveo, es un joven que acaba de empezar a trabajar en una empresa de mudanzas, es tan proactivo como inexperto (y es muy proactivo). Su jefe, Eustaquio, le pide que termine de colgar un cuadro en una habitación. Cuando Clodoveo entra en la habitación se da cuenta de que los únicos recursos que tiene a su alcance son un martillo y un tornillo. Ni corto ni perezoso, Clodoveo coge el martillo y clava el tornillo a martillazos en la pared, finalmente cuelga el cuadro.
El cuadro está colgado, sí, pero al no estar el tornillo diseñado para ser clavado, muy probablemente a medio plazo se terminará cayendo el tornillo, haya que reparar la pared para así poder poner un clavo, que es lo que se tenía que haber hecho desde el principio…

Por lo general, usar herramientas para propósitos para los que no han sido diseñadas hacen que nos enfrentemos a problemas de diversa índole, entre ellos:
* [Corner cases][corner_case]
* Mantenimiento complicado, al no estar en el roadmap de la herramienta dar soporte a la forma de uso que le estamos dando
* Dificulta conseguir soporte sobre nuestra implementación

Muchas veces nos encontramos ante la tesitura de tener que hacer esto por necesidades de negocio, para reducir el [TTM][time_to_market], para evitar bloquear a otros equipos, porque no estamos formados en la tecnología que realmente deberíamos utilizar... Pero en cualquiera de los casos, siempre, **siempre**, deberíamos conseguir tiempo en un futuro próximo para implantar la solución correcta y estudiar qué ha pasado para llegar a ese punto y tratar de poner acciones para que no vuelva a ocurrir.

No seas como Clodoveo, tómate tu tiempo y compra clavos, no perviertas la tecnología!


[unix_philosophy]: https://en.wikipedia.org/wiki/Unix_philosophy
[douglas_mcilroy]: https://en.wikipedia.org/wiki/Douglas_McIlroy
[gke_networking_doc]: https://cloud.google.com/kubernetes-engine/docs/concepts/network-overview
[corner_case]: https://en.wikipedia.org/wiki/Corner_case
[time_to_market]: https://en.wikipedia.org/wiki/Time_to_market
