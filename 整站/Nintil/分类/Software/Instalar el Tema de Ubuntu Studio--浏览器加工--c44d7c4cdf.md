Si aún no sabéis qué es, podéis verlo en el [blog de Cesarius](http://www.cesarius.net/). El aspecto gráfico de Studio me gusta y por eso pongo este HOWTO, recopilado de [aquí](http://www.belutz.net/2007/05/11/installing-ubuntu-studio-theme/).

1\. Edita el /etc/apt/sources.list u Orígenes de Software y agrega:

> **deb http://archive.ubuntustudio.org/ubuntustudio feisty main**

2.Ejecuta esto en consola para bajar la clave GPG

**wget http://archive.ubuntustudio.org/ubuntustudio.gpg -O- | sudo apt-key add -**

3.Actualizamos

>  **sudo apt-get update**

4.SI quieres instalar el tema completo, ejecuta este comando

>  **sudo apt-get install ubuntustudio-look**

Por supuesto, puedes ir a Synaptic e instalar sólo los aspectos que te interesen, como wallpaper, iconos, etc... ![](https://nintil.com/old_assets/2007/05/13-1.png)