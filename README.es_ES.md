(No he estado hablando español por mucho tiempo, así que disculpe cualquier error.)

# Instalar Arch Linux
Arch Linux es un sistema operativo flexible y altamente personalizable. Hemos detectado un problema desconocido. Un elemento disuasorio importante para las personas, sin embargo, es que no hay ninguna interfaz grafica de usuario de instalacion. Esto significa que el terminal debe utilizarse para este proceso. En esta guia, te guiaré a través de instalacion de este sistema operativo desde cero.

# Obtención de medios de installación
Navegue hasta https://www.archlinux.org/download/ y seleccione el espejo se cierra a usted. Seleccione uno de estos. Te llevara a una pagina llena de muchos hipervínculos diferentes. Seleccione el que dice archlinux-x.x.x-x86_64.iso, donde x.x.x es la fech más actual. Tsto descargara un archivo de 600 MB, lo que puede tardar un tiempo.

## Conseguir configuración
Voy a usar virtualbox para este tutorial, pero también debería funcionar para discos duros físicos. Voy a utilizar un disco duro de 20 GB, pero por favor ajuste los valores a cualquier unidad de tamaño que tenga. 

### A un lado corto para virtualbox
Una vez que haya creado la máquina virtual, tendrá que ir a Configuración -> Almacenamiento y, en Controlador: IDE, seleccione el círculo con un signo más y, a continuación, "Elegir disco". Ahora seleccione el archivo ISO que acaba de descargar. 

### A un breve plazo para la instalación en hardware físico. 
Es posible que necesite encontrar los controladores específicos para su [Tarjeta Wifi](https://wiki.archlinux.org/index.php/Configuración de red inalámbrica) y tarjeta gráfica ([NVIDIA](https://wiki.archlinux.org/index.php/Wireless network configuration), [AMD](https://wiki.archlinux.org/index.php/AMDGPU) y [Intel](https://wiki.archlinux.org/index). Si necesita alguno de estos controladores, recomendaría leer la página ENTIRE.)

## Instalacion
### De inicio
A medida que inicie el sistems, seleccione "Boot Arch Linux". En el CLI, aseguerse de que su ethernet este funcionado y teclee `ping www.google.com`. Presione CTRL-C para detenerlo.
### Para partitioning las unidades
#### Sistema de particionamiento basico
Voy a utilizar una sola partición para mi almacenamiento, pero puede administrarlo como desee. Una estrategia común es usar pariciones separadas de /tmp, /mnt y /home. Para empezar, escriba `fdisk -l` e identifique el disco que desea utilizar. En mi caso, es /dev/sda, que es de 20 GB. Tendrá un /dev/loop0, pero puede ignorarlo, es sólo su medio de instalación. Ahora escribiré `cfdisk` para iniciar un software de gestión de parición. En el menú, utilice las teclas de flecha para seleccionar el tipo de etiqueta "dos". Pulse Intro uno en el segundo menú que aparece. En la parte inferior izquierda, cambie el valor a un GB en lo que se muestra. Para mí, el valor mostrado es 20G, así que lo cambiaré a 19G y luego golpearé enter. Seleccione "primario" y, a continuación, presione la flecha izquierda y seleccione la "opción Bootable". Pulse la flecha hacia abajo, para seleccionar el espacio libre, y pulse "nuevo" 1023M debe aparecer como la cantidad. Pulse Intro y vuelva a seleccionar "primario". Ir a la derecha y pulse "escribir", tendrá que escribir "sí" después. Ahora pulsa el botón "salir". Ahora pulsa el botón "salir". Escriba 'fdisk -l' de nuevo para asegurarse de que el cambio realmente guardado. Ahora debería haber un /dev/sda1 y /dev/sda2. Para que las particiones sean utilizables, escriba 'mkfs.ext4 /dev/sda1' y 'mkswap /dev/sda2'. Para utilizar la partición de intercambio, escriba 'swapon /dev/sda2'.
