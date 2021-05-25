Nodos Ros (Publisher - Suscriber) - Comunicación Arduino
==============
*El proyecto consiste en comunicar Ros y Arduino por comunicacion serial, se utiliza el Arduino uno y se trabaja como un nodo de Ros para la comunicación Serial. El Arduino envia 3 datos (un booleano, un entero y un flotante) por tres topics diferentes, los nodos de Ros creados se encargan de recibir y procesar la información utilizando una funcion de pertenecia para obtener el porcentaje que cada dato tiene a una clase (bajo, medio, alto), si cada dato tiene una pertenencia mayor al 50% a una de las clases se dice que pertenece a esa clase y dependiendo de a que clase pertenezaca cada uno de los tres datos que envia el arduino, se envía un valor en grados diferente al arduino para controlar un servomotor.*

![alt text](https://github.com/eliandv1911/Nodos_Ros-Arduino/blob/4da2f36b4e77aebddcadc1e05aac8952442dc556/images/funcion_pertenencia.png)


**NODO "talker_3d.cpp":**
*Este nodo se encarga de recibir los tres datos que son enviados de arduino (bool, int, float) por tres diferentes topics y volverlos a enviar por un topic diferente para que otro nodo los reciba*

**NODO "listener_talker_bool.cpp":**
*Este nodo se encarga de recibir el dato booleano que publica el nodo "talker_3d.cpp", si el dato es "true" va a publicar en un topic que tiene un porcentaje de pertenencia de 100% para alto, por el contrario si es "false" va a publicar en el topic que tiene un porcentaje de pertenencia de 100% para bajo*

**NODO "listener_talker_int.cpp":**
*Este nodo se encarga de recibir el dato entero que publica el nodo "talker_3d.cpp", evalua este dato en las funciones de pertenencia y publica el resultado de la evaluacion en un topic diferente*

**NODO "listener_talker_float.cpp":**
*Este nodo se encarga de recibir el dato flotante que publica el nodo "talker_3d.cpp", evalua este dato en las funciones de pertenencia y publica el resultado de la evaluacion en un topic diferente*

*-------------------------------------------------------------------------------------------------------------------------------------------------------*

***Nota:** para los tres nodos anteriores los topics de cada nodo que publican, estan publicando cadenas con el porcentaje de pertenencia para cada clase (bajo, medio, alto), por ejemplo: "A100%/M0%/B0%"*

*-------------------------------------------------------------------------------------------------------------------------------------------------------*

**NODO "listener_talker_char_1.cpp":**
*Este nodo se encarga de recibir la cadena que viene del nodo "listener_talker_bool.cpp", separa la cadena y dependiendo del porcentaje de pertenencia (si para una clase es mayor a 50% se dice que pertenece a esa clase) asigna una letra que representa a la clase a la cual pertenece (B-A -> bajo-alto), esta letra se publica como un caracter en un topic*

**NODO "listener_talker_char_2.cpp":**
*Este nodo se encarga de recibir la cadena que viene del nodo "listener_talker_int.cpp", separa la cadena y dependiendo del porcentaje de pertenencia (si para una clase es mayor a 50% se dice que pertenece a esa clase) asigna una letra que representa a la clase a la cual pertenece (B-M-A -> bajo-medio-alto), esta letra se publica como un caracter en un topic*

**NODO "listener_talker_char_3.cpp":**
*Este nodo se encarga de recibir la cadena que viene del nodo "listener_talker_float.cpp", separa la cadena y dependiendo del porcentaje de pertenencia (si para una clase es mayor a 50% se dice que pertenece a esa clase) asigna una letra que representa a la clase a la cual pertenece (B-M-A -> bajo-medio-alto), esta letra se publica como un caracter en un topic*

**NODO "listener_char_123.cpp":**
*Este nodo se encarga de recibir los caracteres que publican los tres nodos anteriores y dependiento de los caracteres que lleguen, mediante un arbol de decisiones se le asigna un valor de 0° a 180° a la variable que se va a publicar (la cual corresponde a los grados que se le envian a un servomotor). Este nodo se comunica con el arduino para enviar la informacion procesada (grados del servo)*

![alt text](https://github.com/eliandv1911/Nodos_Ros-Arduino/blob/4da2f36b4e77aebddcadc1e05aac8952442dc556/images/arbol_decisiones.png)

**NODO "serial_node.py" (nodo arduino):**
*Este nodo corresponde al nodo de arduino, el cual se implementa descargando la biblioteca rosserial de arduino. En el código de arduino se coloca a que topics se va a suscribir y se definen los topics que van a publicar; el nodo de arduino se encarga de tomar los datos boleano, entero y flotante de un interruptor y dos potenciomentros. Igualmente el nodo de arduino recibe un valor entero del topic publicador del nodo "listener_char_123.cpp", el cual contiene la informacion de los grados a los que se va a llevar el servomotor*

*-------------------------------------------------------------------------------------------------------------------------------------------------------*

**Configuración para implementar el nodo de arduino:**

<https://biorobotics.fi-p.unam.mx/wp-content/uploads/Courses/contrucci%C3%B3n_de_robots_moviles/2017-1/practicas/prac03.pdf>

*-------------------------------------------------------------------------------------------------------------------------------------------------------*

**Descargar Paquete:**

*1. El paquete se debe descargar dentro de un workspace; por ejemplo: "/home/workspace/src/paquete_pkg"*
*2. Utilizando el terminal se abre el archivo ".bashrc" y se agrega el workspace en caso de que no este agregado:*

```
$ cd

$ sudo gedit .bashrc 
```

- *Al final del archivo .bashrc se agrega la siguiente linea:*

```
source ~/workspace/devel/setup.bash
```

- *luego en una ventana de terminal estando en "/home" nos dirigimos al workspace y compilamos:*

```
$ cd workspace

$ catkin_make
```
*-------------------------------------------------------------------------------------------------------------------------------------------------------*

**Ejecución del proyecto:**
1. *En una ventana de terminal se inicializa el roscore:*

```
$ roscore
```

2. *Teniendo el arduino conectado al computador y teniendo identificado el puerto del arduino, se ejecuta el nodo de arduino:*

```
$ rosrun rosserial_python serial_node.py /dev/ttyACM0
```

3. *Se ejecuta cada uno de los 8 nodos de Ros en una terminal diferente:*

```
$ rosrun paquete_pkg talker_3d
$ rosrun paquete_pkg listener_talker_bool
$ rosrun paquete_pkg listener_talker_int
$ rosrun paquete_pkg listener_talker_float
$ rosrun paquete_pkg listener_talker_char_1
$ rosrun paquete_pkg listener_talker_char_2
$ rosrun paquete_pkg listener_talker_char_3
$ rosrun paquete_pkg listener_char_123
```

4. *Al ejecutar los pasos anteriores podemos ver las conexiones de los nodos ejecutando la siguiente instrucción en otra ventana del terminal:*

```
$ rosrun rqt_graph rqt_graph
```
![alt text](https://github.com/eliandv1911/Nodos_Ros-Arduino/blob/acfa4b49d4b51b779991ef6606279b674600264b/images/nodos_topics_proyecto.png)

*-------------------------------------------------------------------------------------------------------------------------------------------------------*

**Autores**

*Daniel Alberto Cruz Porras*

*Elian Andrés Díaz Vargas*

*Camilo Andrés Sosa Gutierrez*
