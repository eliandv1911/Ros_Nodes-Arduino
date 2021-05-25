# ROS Nodes (Publisher - Suscriber) - Arduino Communication

*On this project we are gonna stablish a communication path between ROS and an Arduino board via Serial Communication, we use the Arduino Uno board and it works as a ROS Node for the Serial Communication. The Arduino Uno board sends 3 data values (1 boolean, 1 integer, 1 float value) via three different topics, ROS Nodes in the computer hadle the receiving and the processing of the information using a belonging function to get the percentage that each data has to a class (Low, Medium, High), if each data has over 50% belonging to one of the classes we could say that that specific data *belongs* to that class and depending on which class belongs each of them three, sent by Arduino Uno board, it sends a value in different grades to the ones sent by the Arduino Uno board to control the servomotor.*

![alt text](https://github.com/eliandv1911/Ros_Nodes-Arduino/blob/1a8b4b3117a02081e3762cdad41c02fc5b0d7e05/images/funcion_pertenencia.png)


**ROS Node "talker_3d.cpp":**
*This node is the one responsible to receive those 3 data sent by the Arduino Uno board (bool, int, float), this node is suscribed to three topics designated by the Arduino Uno board and this node is gonna send them through some new topics designated by itself so the following nodes could receive them.*

**ROS Node "listener_talker_bool.cpp":**
*This node is responsible to receive the boolean data sent by "talker_3d.cpp", if the boolean data is "true" is gonna sent trough the topic that it has a value of 100% for High, on the other hand, if the boolean data is "false" is gonna sent trough the topic that it has a value of 100% for Low.*

**ROS Node "listener_talker_int.cpp":**
*This node is responsible to receive the integer data sent by "talker_3d.cpp", this node evaluates this data in the belonging function and publish the result of this evaluation trough a different topic.*

**ROS Node "listener_talker_float.cpp":**
*This node is responsible to receive the float data sent by "talker_3d.cpp", this node evaluates this data in the belonging function and publish the result of this evaluation trough a different topic.*

*-------------------------------------------------------------------------------------------------------------------------------------------------------*

***Note:** Those three last nodes send their information trough an independant topic for each one, they publish chain with the percentage of belonging for each class (Low, Medium, High), e.g.:"A100%/M0%/B0%"*

*-------------------------------------------------------------------------------------------------------------------------------------------------------*

**ROS Node "listener_talker_char_1.cpp":**
*This node is responsible to receive the chain sent by "listener_talker_bool.cpp", it splits the chain and depending on the belonging percentage (in case the class belonging percentage is greater than 50%, we could say that out data received belongs to that class, whether we still have a small percetage on the other classes or don't), we assign a letter corresponding to the class this data belongs to (B-A -> bajo-alto), this letter is published as a char type value in the new topic.*

**ROS Node "listener_talker_char_2.cpp":**
*This node is responsible to receive the chain sent by "listener_talker_int.cpp", it splits the chain and depending on the belonging percentage (in case the class belonging percentage is greater than 50%, we could say that out data received belongs to that class, whether we still have a small percetage on the other classes or don't), we assign a letter corresponding to the class this data belongs to (B-M-A -> bajo-medio-alto), this letter is published as a char type value in the new topic.*

**ROS Node "listener_talker_char_3.cpp":**
*This node is responsible to receive the chain sent by "listener_talker_float.cpp", it splits the chain and depending on the belonging percentage (in case the class belonging percentage is greater than 50%, we could say that out data received belongs to that class, whether we still have a small percetage on the other classes or don't), we assign a letter corresponding to the class this data belongs to (B-M-A -> bajo-medio-alto), this letter is published as a char type value in the new topic.*

**ROS Node "listener_char_123.cpp":**
*This node is responsible to receive those three char type values sent by the three last explained nodes and depending on the characters it receives, by means of a decision three, a to-be published data is assigned a value between [0° - 180°] (this values corresponds to the degrees our servo is gonna be set to). This node talks to the Arduino Uno board to send the processed information (servo degrees).*

![alt text](https://github.com/eliandv1911/Ros_Nodes-Arduino/blob/f59d9153f8f082b6ea44b402866ecb51ece63f6f/images/arbol_decisiones.png)

**ROS Node "serial_node.py" (Arduino Uno Node):**
*This node is implemented by downloading the rosserial arduino library. On this Arduino code we declare which topics out arduino is gonna be listening to and also which ones our Arduino Uno board is gonna be publishing to. The Arduino Node is gonna take those 3 data values (bool, int, float) from 1 switch and 2 potentiometers. Likewise, the arduino node receives an integer value from the topic publisher of the node "listener_char_123.cpp", which contains the information of the degrees to which the servomotor is going to be taken to.*

*-------------------------------------------------------------------------------------------------------------------------------------------------------*

**Configuration to implement the Arduino Node:**

<https://biorobotics.fi-p.unam.mx/wp-content/uploads/Courses/contrucci%C3%B3n_de_robots_moviles/2017-1/practicas/prac03.pdf>

*-------------------------------------------------------------------------------------------------------------------------------------------------------*

**Package Downloading:**

*1. Package must be downloaded inside a workspace, e.g.:  "/home/workspace/src/paquete_pkg"*
*2. Using the terminal we open the file ".bashrc" and add the workspace in case it hasn't been added:*

```
$ cd

$ sudo gedit .bashrc 
```

- *At the end of the file ".bashrc" we should add the following line of code:*

```
source ~/workspace/devel/setup.bash
```

- *Then, on another terminal window on the "/home" location, we enter to the workspace and compile it:*

```
$ cd workspace

$ catkin_make
```
*-------------------------------------------------------------------------------------------------------------------------------------------------------*

**# Project Execution**
1. *First we open a terminal window and initialize roscore:*

```
$ roscore
```

2. *We connect the Arduino Uno board to the computer and with the port address of it, we execute the Arduino node:*

```
$ rosrun rosserial_python serial_node.py /dev/ttyACM0
```

3. *Then we open a terminal for each node we have, we have 8 c++ nodes so we open 8 terminals and run one of these lines on each terminal:*

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

4. *After launching every node explained before, we could see the connections between them executing the folllowing command on a separate terminal:*

```
$ rosrun rqt_graph rqt_graph
```
![alt text](https://github.com/eliandv1911/Nodos_Ros-Arduino/blob/acfa4b49d4b51b779991ef6606279b674600264b/images/nodos_topics_proyecto.png)

*-------------------------------------------------------------------------------------------------------------------------------------------------------*

**Autores**

*Daniel Alberto Cruz Porras*

*Elian Andrés Díaz Vargas*

*Camilo Andrés Sosa Gutierrez*
