*This version is updated by Hebah ElGibreen based on [patrolling_sim](https://github.com/gennari/patrolling_sim) to introduce dynamic environment and SAUDE Algorithm (written in the code as ODTA) in addition to ontology with uncertainty.*

This package is to be downloaded on the robot and connected to the [PC version](https://github.com/hebahG/sim_patrolling_wth_Ontology_PC) that act as a bridge since the prolog is not compatible with Turtlebot processor (details in **ElGibreen, Hebah, and Kamal Youcef-Toumi. "Dynamic task allocation in an uncertain environment with heterogeneous multi-agents." Autonomous Robots (2019): 1-26.**.

**===================================================**

**Setup Before Run**

**===================================================**

1- Install the follwing packages:
	- tcp_interface
	- turtlebot_setp

2- Build the project folder.
> You will get an error becuase of TCP so source dev/setup.bash first then build again (catkin_make).

3- Make sure the robot names start with "robot" then its number, it is better the start the physical robots with 0 (i,e, name robot0...)

4- Change the ip adresses at  UDPpeers.cfg file in tcp_interface to include all the ips of the robots.

5- Change the settings in all turtlebot_setup files from hokuyi to asus_xtion_pro since I have a camera instead of laser in my robots ==> actually I just created fake 2d laser which is supported by Kinect.

6- Changed settings files to match your experiment:
	- navigation.launch
	- mapping.launch
7- Creating your own map:
> It is VERY IMPORTANT to note the initial position & direction of the robot started to build the map from becuase in the real robots this will be the initial position and orintation.


**==========================================================**

**How to run the MRP agents on different robots ([source](https://github.com/gennari/patrolling_sim))**
**==========================================================**

1- Prepare your robots so that they can navigate in the environment with move_base. 

2- Configure the network so that they can communicate each other.

3- Copy the 'patrolling_sim' and 'tcp_interface' packages on all the robots.

4- Configure the file tcp_interface/config/UDPpeers.cfg adding one line for each host IP in the team. 
	> Leave the port numbers as 9100 9110 in all the lines.
Example:
192.168.56.1	9100	9110
192.168.56.101	9100	9110
192.168.56.102	9100	9110

You can use the same file with all the IPs in all the machines.

5- Modify the launch file robot_template.launch in patrolling_sim/launch to use your own robot. 
The file have to run all the robot node for use move_base and amcl. The launchfile have to use an arg named robotname for the namespace.
Patrolling node sends necessary commands to robots through /robot_name/move_base action server, read the robot pose from the topic /robot_name/amcl_pose and provides the map with the map_server on the topic /map.

6- Start the script 'start_experiment_distributed.py' on all the robots.

7- On each robot, select the map, the number of robots, the algorithm and other info in the launcher. Choose the proper network tcp_interface (e.g. wlan0) corresponding to the robot network.
	
	Put ID equal to its name
	False to monitor
	Robot is real
