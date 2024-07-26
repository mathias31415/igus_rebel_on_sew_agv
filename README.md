# igus_rebel_on_sew_agv

## Table of Contents
1. [Overview](#overview)
2. [Video](#video)
3. [How to use the AGV and the robot](#howto)

<a name="overview"/>

## Overview
<img src="doc/sew_agv_rembg.png" alt="sew_agv" height="400"/> 

This is our main repository for the Igus Rebel robot (open source variant) on a SEW AGV. It was developed by Hannes Bornemann, Robin Wolf, and Mathias Fuhrer as part of a project at Karlsruhe University of Applied Sciences.

All the ROS2 packages related to this project can be found in the [ros2-packages](https://github.com/mathias31415/igus_rebel_on_sew_agv/tree/main/ros2-packages) folder.

### [sew_maxo_mts_ros2](https://github.com/RobinWolf/sew_maxo_mts_ros2/tree/dev)
This repository contains the code for controlling the SEW AGV using an Xbox joystick or keyboard.

### [igus_rebel_ros2_docker](https://github.com/mathias31415/igus_rebel_ros2_docker)
This repository contains the code for controlling the Igus Rebel robot mounted on the AGV via RViz.

### [sew_maxo_mts_and_igus_rebel](https://github.com/RobinWolf/sew_maxo_mts_and_igus_rebel)
This repository contains the Gazebo simulation for simulating the AGV with the robot picking boxes in a simulated warehouse.

For more detailed documentation, please refer to the README files of the respective repositories or visit our (german) [documentation]().


<a name="video"/>

## Video
This [YouTube video]() shows the results of our project work:

TODO

<a name="howto"/>

## How to use the AGV and the robot
Um das Agv mit dem roboter zu nutzen, muss das agv und der roboter in der vorgegebenen reihenfolge eingeschalten werden. Auf dem Rasperry pi, der sich im fuß des roboters befindet wird automatisch alles eingerichtet und die ros packages gelauncht. WEnn der raspi fertig gebootet hat kann man sich mit einem weiteren pc (mit ubuntu betriebssystem) mit dem vom raspi aufgemachten wifi verbinden. Das wifi heist `AGV` und das passwort ist `agv12345`. Auf dem pc können nun die beiden docker container gebaut und gestartet werden. Während dem bauen wird eine Internetverbindung benötigt. Erst nach dem starten des docker containers sollte man sich mit dem `AGV` wifi verbinden. Im `igus_rebel_ros2_docker` docker container muss dann RViz gestartet werden, im `sew_maxo_mts_ros2` docker container muss das launchfile für den joystick gelauncht werden. Um dan Roboter und das agv wieder auszuschalten, muss wieder die vorgegebene reihenfolg eingehalten werden.

### 1. AGV und Roboter einschalten

<img src="doc/switches_back.png" alt="switches" height="400"/> <img src="doc/switches_robot.png" alt="reboot" height="250"/>

1. Stelle sicher, dass roboter hauptschalter ausgeschaltet ist (Achtung: Ausgeschaltet ist die Schalterstellung senkrecht ( | ) und eingeschaltet waagrecht ( – ))
2. pull the agv emergency-stop
3. turn on the AGV by pressing and holding the green and blue button for a few seconds
4. The display is now turning on and the green button is lit up.
5. Ziehe den roboter emergency stop falls er gedrückt ist
6. Schalte den roboter haptschalter ein
7. Sobald der raspi vollständig gebootet hat ist das wifi `AGV` sichtbar, der grüne button leuchtet nichtmehr und auf dem display steht unten rechts in einem blauen kästchen `RC`. Der roboter summt leise und aktiviert die einzelnen achsen, was durch ein klicken wahrnehmbar ist.
8. Wenn es zu fehlern kommt, kann der raspi über den reboot knopf im roboterfuß neu gestartet werden. Ein reboot ist auch notwendig, wenn der nothalt des roboters betätigt wurde.

### 2. Docker container bauen und starten
1. Clone die beiden repos:
```
git clone https://github.com/mathias31415/igus_rebel_ros2_docker.git
git clone https://github.com/RobinWolf/sew_maxo_mts_ros2.git
```
(Stelle sicher, dass du dich auf dem richtigen branch befindest (mit `git checkout <branchname>`))

2. Baue und starte die beiden docker container, indem du in das entsprechende verzeichnis navigierst (mit `cd`) und dann die skripte zum bauen und starten des containers ausführst. Dafür musst du mit dem internet verbunden sein.
```
./build_docker.sh
./start_docker.sh
```
3. Gehe aus dem jeweiligen docker container wieder heraus, indem du CTRL + C drückst. Der dockercontainer läuft im hintergrund weiter.


### 3. Mit dem wifi des raspberry pi verbinden
Verbinde deinen PC mit dem `AGV` wifi, das vom raspi aufgemacht wird. Das passwort ist `agv12345`.

### 4. RViz im `igus_rebel_ros2_docker` docker container starten
1. Verbinde dich wieder mit dem `igus_rebel_ros2_docker` docker container
```
docker exec -it igus_rebel bash
```
2. Source 
```
source install/setup.bash
```
3. Starte RViz 
```
ros2 launch irc_ros_bringup rviz.launch.py
```
Du kannst nun über RViz den Roboterarm bewegen


### 5. Joystick im `sew_maxo_mts_ros2` docker container nutzen
1. Verbinde dich wieder mit dem `sew_maxo_mts_ros2` docker container
```
docker exec -it sew_navigation bash
```
2. Source 
```
source install/setup.bash
```
3. Starte das Joystick launchfile 
```
ros2 launch sew_agv_navigation joystick.launch.py
```

### 6. AGV und Roboter ausschalten
1. Dücke den roboter nothalt, um die 24V Spannungsversorgung des roboters wegzunehmen (die Logigspannung bleibt erhalten)
2. Schalte den Roboter hauptschalter aus
3. push the AGV emergency-stop
4. turn of the AGV by pressing and holding the green and blue button for a few seconds
5. The display turns off and the buttons stop lighting up.



