|  Información del documento |
|---|
| Versión: 1 |
| Autor: Rodrigo Castro |
| Fecha de creación: 07/12/21 |
|Fecha de Actualización 07/12/21 |


# Borrado seguro de información

Borrar información de manera segura (también conocido como data destruction, secure erase, secure data wiping, entre otros) 

Para hacer irrecuperable los datos se existen tres métodos

- **Degaussing**: consiste en desmagnetizar los medios, este metodo solo es efectivo para dispositivos que almacenan los datos de forma magnética como antiguos dispositivos como diskette o HDD, pero no funciona con las unidades flash o los SSD, causa que los dispositivos queden inservibles y tiene un impacto en el medio ambiente.

 - **Destrucción física**: consiste en destruir físicamente el medio que contiene los datos, haciendo inutilizable el medio o la recuperación de los datos, sin embargo deja el medio inutilizable y tiene un impacto en el medio ambiente.

- **Mediante software**: Consiste en usar software que realiza un proceso de eliminación que va más allá de la básica eliminación de archivos, donde se eliminan los punteros de los datos sin eliminar los datos. Es la opción sugerida en este documento


---

## Software

Se recomienda que el software para la eliminación de datos debé permitir la selección de un estándar específico y verifique que el método de sobrescritura fuera exitoso y haya eliminado datos en todo el dispositivo. Se recomienda el uso de SharedOS

### ShredOS

ShredOS es una distribución de Linux liviana que se puede usar desde una USB (BIOS o UEFI) con el único propósito de borrar de forma segura todo el contenido de discos utilizando el programa nwipe, ShredOS siempre proporcionará el último nwipe en un kernel de Linux actualizado para que sea compatible con hardware moderno admitiendo procesadores de 32 bits o 64 bits. 

[Descarga SharedOS][l01]

[Documentación oficial de SharedOS][l02]

## Creando USB live

Una vez descargada la imagen de SharedOS, utilizaremos el programa balenaEtcher que esta disponible para Windows, Linux y macOS se puede descargar desde el siguiente [enlace][l03].

insertamos el dispositivo USB en el que se instalara SharedOS y ejecutaremos balenaEtcher.

![belenaEtcher][i02]
 
Seleccionamos la opción ***Flash from file***

![Flash from file][i03]

Seleccionamos el archivo de imagen de SharedOS

![Selección archivo de imagen SharedOS][i04]

**Nota**: Asegúrese de descargar el archivo de imagen correcto, el archivo `.img` es recomendable para generar en una live USB y el archivo `.iso` es recomendable apra quemar en un live CD 

Verificamos el dispositivo USB a utilizar, de no ser el que necesitamos podemos cambiarlo pulsando sobre la opción ***Change***

![Verificando el dispositivo USB][i05]

Presionamos sobre el botón ***Flash!***

![Flash][i06]

Esperamos a que termine el proceso

![Flashing][i07]

Una vez terminado el proceso retiramos el dispositivo USB.

## Iniciando el proceso

Apagamos el equipo donde usaremos ShareOS para eliminar archivos, e iniciamos el sistema desde el dispositivo USB conectado

**Nota**: En caso de sistemas operativos Windows en versiones 8, 10 y 11 dado a que el sistema en si no se apaga para dar una sensación de ser mas rapido, evita que los discos se puedan montar por lo que tenemos dos opciones

- Abrir CMD como administrador y ejecutar el comando `powercfg -h off`
- Abrir CMD como administrador y ejecutar el comando `shutdown -r -fw -t 0`

### Iniciando ShredOS

Al iniciar veremos la siguiente pantalla

![Pantalla de inicio Shared OS][i08]


Presionamos la tecla `m` para seleccionar el metodo de eliminación.

Se sugiere uasr el estándar DoD 5220.22M

![Seleccionar método de eliminación][i09]

Seleccionamos la unidad o unidades de disco en las que deseamos eliminar los datos y presionamos la tecla espacio.

![Seleccionar unidades a borrar][i10]

Una vez seleccionadas las unidades presionar la tecla shift + s, el proceso iniciara

![Eliminación en proceso][i11]

Dependera del tamaño del disco el tiempo que tardara en eliminar los datos.

![Eliminación completa][i12]

Una vez terminado presionamos enter y nos mostrara un resumen del procedimiento realizado.

![Resumen del proceso][i13]

Copiamos este reporte para incluirlo en un informe técnico.

### Copiar archivo de log del proceso realizado

***Nota***: el reporte se genera una vez terminado el proceso y se debe copiar antes de salir del sistema.

Podemos insertar una usb para copiar el reporte, utilizamos el comando para lostar dispositivos conectados

~~~bash
lslbk
~~~

![Listar dispositivos conectados][i14]

Identificamos el dispositivo, en este caso es el `sdb1` creamos un directorio para montar el volumen con el comando 

~~~bash
mkdir /media/usb
~~~ 

Montamos el volumen con el comando 

~~~bash
mount /dev/sdb1 /media/usb
~~~

Listamos el contenido del directorio de ShredOS para ver los archivos con el comando

~~~bash
ls
~~~

Veremos un archivo con la estructura `nwipe_log_<fecha>-<hora>`

***Nota***: Si hemos realizado varios procesos se crearan varios archivos `nwipe_log_` variando la fecha o la hora

Procedemos a copiar el reporte o reportes con el comando 

~~~bash
cp /nwipe_log* /media/usb
~~~

![Comandos][i15]

Verificamos que los archivos esten en el dispositivo USB con el comando 

~~~bash
ls /media/usb/
~~~

![Verificando los archivos copiados][i16]

Desmontamos el dispositivo USB con el comando

~~~bash
cd /media/;umount usb
~~~

![Desmontar volumen][i17]

Desconectamos el dispositivo usb con los reportes y finalizamos el sistema, ahora el disco se encuentra libre de datos y no es posible recuperar información. 

<!-- lista de imagenes -->

[i02]: /imagenes/02-belenaEtcher.png "belenaEtcher"
[i03]: /imagenes/03-flhas-from-file.png "Opción Flash from file"
[i04]: /imagenes/04-seleccion-archivo-de-imagen.png "Selección archivo de imagen SharedOS"
[i05]: /imagenes/05-verificar-dispositivo.png "Verificando el dispositivo USB"
[i06]: /imagenes/06-Flash.png "Flash"
[i07]: /imagenes/07-flashing.png "Flashing"
[i08]: /imagenes/08-shredos.png "Pantalla de inicio SharedOS"
[i09]: /imagenes/09-seleccionar-metodo.png "Seleccionar método de eliminación"
[i10]: /imagenes/10-unidades-seleccionadas.png "Seleccionar unidades a borrar"
[i11]: /imagenes/11-wiping.png "Eliminación en proceso"
[i12]: /imagenes/12-wiping-finish.png "Eliminación completa"
[i13]: /imagenes/13-resume.png "resumen del proceso"
[i14]: /imagenes/14-listar-dispositivos-conectadso.png "Listar dispositivos conectados"
[i15]: /imagenes/15-comands.png "Comandos usados"
[i16]: /imagenes/16-verificando-archivos-usb.png "Verificando los archivos copiados"
[i17]: /imagenes/17-unomunt.png "Desmontar volumen"


<!-- lista de links -->

[l01]: https://github.com/PartialVolume/shredos.x86_64/releases "SharedOS Releases"
[l02]: https://github.com/PartialVolume/shredos.x86_64 "SharedOS"
[l03]: https://github.com/PartialVolume/shredos.x86_64 "sito de balenaEtcher"