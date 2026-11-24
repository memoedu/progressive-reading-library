Os dejo una guía para hacer lo que pone en el título. Yo seguí estos mismos pasos para pasar de Windows 8.1 a Ubuntu 15.04 con éxito.

Seguramente habrás visto por ahí guías para versiones antiguas de Windows, o hayas leído historias oscuras con el UEFI o problemas de instalación de GRUB, y es cierto que por poder pueden pasar cosas: estamos hablando de instalar Ubuntu junto a un Windows que acaba de salir, y no hay tantas instalaciones hechas con éxito como sí las hay con 8.1. En cualquier caso, la guía más segura para instalar Ubuntu es la que doy aquí, y seguramente no haya problema alguno.

El paso cero es actualizar a Windows 10. Windows es especialito y al actualizar se espera que él sea el único SO en disco, así que mejor facilitarle las cosas y actualizar desde un disco sin toquetear con los pasos siguientes. Luego, hay que hacer copia de seguridad. (Yo no la hice. Espero que eso dé algo de confianza en esta guía.)

El primer paso es bajar la ISO de [Ubuntu 15.04](http://www.ubuntu.com/download/desktop) (Importante que sea esta versión o posteriores, o dará problemas con el UEFI!), 64 bits, salvo que os vaya el sado y hayáis instalado Windows 10 en un Pentium 3. Luego hay que seguir [esta guía](http://www.ubuntu.com/download/desktop/create-a-usb-stick-on-windows) para instalarlo en un pendrive, vale uno de 1 GB.

Ahora, hay que defragmentar el disco con la herramienta correspondiente (para evitar pérdidas de datos, y facilitar la operación siguiente)

Hecho eso, hay que hacer el particionado desde Windows. Por poder podrías hacerlo con gparted desde Ubuntu, y esta era la opción tradicional, pero esta otra es más segura. De modo que en windows invocamos vía Cortana al gestor de particiones,

![](https://nintil.com/old_assets/2015/08/11-1.png)

Ahí se coge la partición más grande (Será la que corresponda a la de Windows, y no a particiones de recuperación o arranque) y click derecho, hay que darle a reducir volumen. Saldrá un selector de tamaño numérico en kilobytes para elegir el tamaño de la partición

![](https://nintil.com/old_assets/2015/08/11-2.gif)

Pues si queremos 100 GB, metemos 102400 ahí.

Una vez empequeñecida queda un hueco. Cerramos eso, y ahora hay que ir a opciones de energía y deshabilitar la opción de arranque rápido. Según cuentan por ahí, esto ya no es tan necesario si no vas a pasar archivos de Windows a Ubuntu usando Nautilus desde Ubuntu, pero por si acaso vamos a hacerlo. En opciones de energía (O algo así se llamaba :), hay que DESMARCAR la casilla que sale marcada en esta foto, tal que quede SIN el tick. Guardamos cambios. Reinicia el PC y apaga el PC, por si acaso.

![](https://nintil.com/old_assets/2015/08/11-3.jpg)

Ahora, con el Pendrive conectado, enciende el PC. Al reiniciar, hay que entrar en la BIOS (Suele haber que dejar pulsada la tecla F9 o F12, y cuidado con lo que tocáis porque os podéis cargar el PC si le dáis a cosas que no son) y desactivar Secure Boot, QuickBoot/FastBoot e Intel Response Technology SI LAS HUBIERA (Yo no tenía ninguna de ellas, por ejemplo). Luego, en Boot order, hay que poner el UEFI: USB (o algo que se llame parecido) el primero de la lista.

Guardamos y salimos (Save changes and exit suele llamarse la opción)

Ahora aparecerá Ubuntu, hay que darle a Try Ubuntu without installing, y una vez que carga, hay que comprobar que todo funciona bien. Entonces, se procede a la instalación con el instalador del escritorio de forma normal, eligiendo la opción 'Install Ubuntu alongside Windows Boot Manager'. Esto lo hará todo por nosotros de forma fácil, pero nos meterá también una partición swap de tantos gigas como tengamos de RAM (si no la queremos, una vez instalado, hay que ejecutar sudo swapoff en ubuntu, reiniciar de nuevo con el Pendrive, editar particiones desde Ubuntu, eliminar la swap, agrandar la partición de ubuntu, apagar, quitar pen y reiniciar).

Y ya está. En gran medida esta guía sale de [aquí](https://help.ubuntu.com/community/UEFI) y de otras dispersas por la red.

Otras cosas que sé hacer y que pueden hacer falta tras instalar Ubuntu y que cuesta encontrar: cómo activar el subwoofer de portátiles Lenovo, si a alguien le hace falta que pregunte.

-   ![Humberto Israel](https://www.gravatar.com/avatar/a911d3bcc2f7c4e99e76c30dd648ef29?d=identicon) **Humberto Israel** 2015-12-06T20:00:12Z
    
    > Habia escuchado que Ubuntu no se podía instalar con Windows 10 porque Windows no se apagaba por completo, sino que hibernaba para arrancar más rápido, y bueno, yo cuando meti mi memoria y arranque desde ahí, ya fuera que le diera probar Ubuntu o instalar, me salían varias lineas de código y de ahí no avanzaba, que puedo hacer?
    
-   **[2015 in Nintil: Top posts | Nintil](https://artir.wordpress.com/2015/12/31/2015-in-nintil-top-posts/)** 2015-12-31T11:58:24Z
    
    > \[…\] Instalar Ubuntu con Windows 8.1/10 on how to, well, install Ubuntu alongside Windows. \[…\]
    
-   ![benjamin](https://www.gravatar.com/avatar/a67eccfea09e60cf4eddc51b32849853?d=identicon) **benjamin** 2016-12-07T17:58:06Z
    
    > hola a mi no me aparece la opcion de installa Ubuntu alongside windows boot maneger, como le hago para verla ??