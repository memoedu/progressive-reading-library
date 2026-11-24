Se acerca la release de Feisty y mañana los servidores estarán saturados de descargas. Podemos pedir los CD'S por ShipIt o esperar un par de días, pero si somos impacientes, como yo, podemos comenzar el proceso de descarga hoy, a día 18. ¿Cómo?: con rsync, un programa que se encarga de todo.

El plan consiste en:

-   Bajar la imagen de hoy
-   Bajar mañana los cambios entre la de hoy y la final.

**rsync -vPz rsync://cdimage.ubuntu.com/cdimage/daily-live/current/feisty-desktop-i386.iso .**

El punto del final es necesario.

Ese comando hará que la imagen actual de Feisty se baje. Cuando salga la versión final, queremos que se bajen solo los pocos paquetes que se hayan cambiado. Por ello, ejecutamos esto tras ir a la carpeta donde estaba la imagen bajada previamente:

**mv feisty-desktop-i386.iso ubuntu-7.04-desktop-i386.iso**

**rsync -vPz rsync://releases.ubuntu.com/feisty/ubuntu-7.04-desktop-i386.iso .**

Nuevamente el punto final es necesario.

Cuidado; no son los mismos rsync. El de arriba es de cdimage.ubuntu.com/daily-live/current y el de abajo es de releases.ubuntu.com/feisty.

Una vez realizado los dos últimos comandos, se bajarán las diferencias entre la imagen anterior y la nueva y se aplicarán, es decir, que en el día en el todo el mundo esté bajando los 700 megas y pico de Feisty, nosotros solo necesitaremos unos pocos minutos(a lo sumo una hora) para tener nuestra Feisty Fawn Final Version y de paso, no sobrecargar los servidores de Canonical.

Fuente: [ubuntu.wordpress.com](http://ubuntu.wordpress.com/2007/04/17/start-downloading-feisty-now-and-get-it-faster-on-release-day/)