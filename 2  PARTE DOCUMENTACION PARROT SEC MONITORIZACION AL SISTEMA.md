2da  PARTE DOCUMENTACIÓN PARROT SEC 

**MONITORIZACIÓN AL SISTEMA**

**SAR:** Es una herramienta muy potente, ya que permite detectar cuellos de botella al permitir visualizar
lo que está ocurriendo en el sistema y almacenar la información sobre la carga y el estado en ficheros históricos, que podrán recuperarse posteriormente para analizar el comportamiento del sistema. Consta de dos órdenes complementarias: *sadc* y *sar*.

*sadc*: recoge los datos relacinados con la actividad del sistema y pasa estos datos a sar
o construye un registro en formato binario.

*sar*: lee los datos binarios que recoge sadc y los traduce a un formato legible.
Para capturar el comportamiento de forma interactiva usaremos la orden sar, que se encargará
de lanzar sadc y mostrar la información obtenida con el colector de datos. 

Un ejemplo de su uso es: 

``` 
  sar u 2 5
  
```

Mostrará 5 tomas de datos de la utilización de la CPU separadas por 2
segundos una de otra.

También permite que los datos sean almacendos en un archivo, por ejemplo: 

```
  sar I 14 o iterr 2 10 , 

```
muestra estadísticas de la interrupciones del sistema, capturándolos 10 veces cada 2 segundos y almacenando los resultados en un fichero llamado iterr.

Usando la instrucción de la forma: sar A , se obtendrá la información de la actividad obtenida en el fichero de captura del día en curso.

Para generar archivos de datos usaremos sadc, indicando el tiempo entre muestras, el número de muestras que se desea recoger y el archivo en el que se desea realizar la captura de datos.

Un ejemplo de su uso es: 

```
  /usr/lib/sysstat/sadc 1 10 /tmp/datafle . 
  
```

Se recogerán 10 muestras separadas por un segundo de diferencia y se almacenará el resultado en el archivo binario datafle, en la carpeta /tmp.

Si el nombre del fichero no se incluye, sadc almacena la actividad del sistema en el archivo /var/log/sysstat/sadd, donde dd indica el número del día en el que se ha realizado el registro de datos.

Admite el uso de modifcadores, algunos de ellos son:
d: captura de la actividad de los discos. Por defecto esta actividad no se captura para evita archivos hitóricos demasiado largos.
I: captura de todas las interrupciones del sistema.

También sar admite el uso de modifcadores para generar informes de diferentes
características, algunos de ellos son:
u: utilización del procesador.
B: paginación de la memoria virtual.
c: creación de procesos.
b: transferencias con E/S.
d: transferencias por cada disco.
I: sistema de interrupciones.
n: conexión de red.
q: carga media del sistema.
r: sistema de memoria.
w: cambios de contexto.
W: intercambio de memoria (swapping)
A: toda la información almacenada.

**MONITORIZACIÓN DE PROGRAMAS**

Es una técnica utilizada para obtener información sobre la ejecución de programas, en particular si deseamos conocer qué parte del código de un programa es la que más tiempo de ejecución consume o cuál es la secuencia de llamadas entre procedimientos.
Se desarrolla por medio de tres etapas principales:

 1. El código fuente del programa a estudiar se debe compilar, incluyendo las opciones necesarias para su monitorización, lo que se denomina instrumentalización.
 2. Una vez instrumentalizado, el programa se ejecuta para poder recoger los datos de la monitorización.
 3. Por último, se ejecuta la herramienta adecuada para leer la información recogida.

En el ejemplo que estudiaremos aplicamos la técnica de monitorización a un fichero escrito en lenguaje C, para ello necesitamos compilarlo usando gcc con la opción pg, que genera código extra que permite el análisis con la herramienta gprof. Para ello usaremos la secuencia siguiente, suponiendo que deseamos estudiar un programa denominado prueba.c:

```
  gcc prueba.c o prueba pg g
  prueba
  gprof prueba > prueba.gprof

```

La primera línea instrumentaliza el programa, almacenando la salida en el archivo indicado tras el modifcador o. Se incluyen también las opciones pg, para poder usar gprof, g para obtener información válida para el depurador del sistema operativo (GDB).
La información se almacena en el fichero prueba.gprof, al que se puede acceder usando un editor de textos.


MONITOR DEL SISTEMA 

Un sistema Gnu/Linux se puede monitorizar utilizando la herramienta de administración denominada Monitor del sistema, la cual permite monitorizar los procesos que se están ejecutando en el sistema y el uso que están haciendo de los recursos. Para facilitar su uso presenta una serie de pestañas:

  Procesos: muestra los procesos activos y cómo se relacionan unos con otros.
  Recursos: presenta la evolución del consumo.

Ofrece información como la carga media en los últimos 1, 5 y 15 minutos. Los procesos aparecen en una tabla en la que, por defecto se muestra: 
 -el nombre del proceso 
 -su estado
 -el porcentaje de uso de CPU
 -su prioridad
 -su identifcador y 
 -la memoria en uso .
Utilizando el menú ver, con la pestaña Procesos seleccionada, podemos seleccionar el tipo de procesos que deseamos monitorizar. Además podemos manipular procesos usando el menú contextual de cada uno de ellos. En particular podemos detener y continuar un proceso, forzar la terminación normal de un proceso o su muerte, cambiar su prioridad. Podemos acceder al Mapa de memoria de un proceso, donde obtendremos información acerca de los segmentos de memoria utilizados: direcciones, tamaño y otras características. Por último, podemos conocer los archivos abiertos por un proceso, obeniendo información del descriptor, el tipo y objeto de los archivos abiertos por el proceso.

En la pestaña Recursos podemos observar algunos gráficos que representan la evolución de la CPU la Memoria de intercambio y la Red. Por su parte, en la pestaña "Sistemas" de archivos se presenta infomación específca de los dispositivos montados, de su directorio de montaje , tipo y memoria total, libre, disponible y usada. Por último indicar que en el menú Editar podemos acceder a Preferencias, desde donde podemos confgurar el tiempo de refresco de la información, los parámetros a monitorizar y la presentación de ciertos elementos del monitor.
