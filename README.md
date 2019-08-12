This version is updated by Hebah ElGibreen to introduce dynamic environment and SAUDE Algorithm (written in the code as ODTA)in addition to ontology with uncertainty.

This package is to be downloaded on the robot and connected to the PC version that act as a bridge since the prolog is not compatible with Turtlebot processor (details in ""ElGibreen, Hebah, and Kamal Youcef-Toumi. "Dynamic task allocation in an uncertain environment with heterogeneous multi-agents." Autonomous Robots (2019): 1-26."".

==========================================================
		       Setup Before Run
==========================================================
First install the follwing package 
https://github.com/gennari
patrolling_sim
tcp_interface
turtlebot_setp

After that, follow the instruction in each package from https://github.com/gennari and make sure that everything is set

Then build the project folder (you will get an error becuase of TCP so source dev/setup.bash first then build again (catkin_make))

Make sure the robot names start with "robot" then its number, it is better the start the physical robots with 0 (i,e, name robot0...)

Also chnage the ip adresses at  UDPpeers.cfg file in tcp_interface to include all the ips of the robots.

Also change the settings in all turtlebot_setup files from hokuyi to asus_xtion_pro since I have a camera instead of laser in my robots ==> actually I just created fake 2d laser which is supported by Kinect.

Changed settings files:
navigation.launch
mapping.launch
=====================
:: Turtlebot speed Change ::
=====================
To make sure that the navigation is slow enough to recognize faces in the right time change the turtlebot parameters from the dwa_local_planner_params.yaml which is found in the config directory of the turtlebot_navigation package..
::ORIGINAL RELATED PARAMETERS::
  max_vel_x: 0.5
  min_vel_x: 0.0
  max_rot_vel: 5.0  
  min_rot_vel: 0.4 #or 0.1
  rot_stopped_vel: 0.4
  acc_lim_theta: 2.0
  acc_lim_x: 1.0
  acc_lim_y: 0.0
=====================
For my experement: real robots capabilities::
	Robot_0: max_vel_x: 0.15 (ontology as 15), acc_lim_theta: 1.5 (ontology as 1.5), mass 2.4 (ontology 2.4)
	Robot_1: max_vel_x: 0.20 (ontology as 20), acc_lim_theta: 1.75 (ontology as 1.75), mass 2.4 (ontology 2.4)
	Robot_2: max_vel_x: 0.25 (ontology as 25), acc_lim_theta: 2.0 (ontology as 2.0), mass 2.4 (ontology 2.4)
=====================
:: Turtlebot Radus Change ::
=====================
To reduce the restriction on hitting obsticals so that local planner " turtlebot_navigation/param" can be found, the robot and inflition raduios needs to be changed as follows::

robot_radius=0.10, inflation_radius=0.5, cost_scaling_factor=5.0, path_distance_bias: 64.0,goal_distance_bias: 24.0, occdist_scale=0.1.
=================
Creating mu own map::
============
I created my map and all information are commented in map file,, but it is VERY IMPORTANT to note the initial position & direction of the robot started to build the map from becuase in the real robots this will be the initial position and orintation!!


==========================================================
       How to run the MRP agents on different robots
==========================================================

1. Prepare your robots so that they can navigate in the environment with move_base. Configure the network so that they can communicate
each other.

*** TODO: check topic and frame names ***


2. Copy the 'patrolling_sim' and 'tcp_interface' packages
on all the robots.


3. Configure the file tcp_interface/config/UDPpeers.cfg adding
one line for each host IP in the team. 
Leave the port numbers as 9100 9110 in all the lines.

Example:

192.168.56.1	9100	9110
192.168.56.101	9100	9110
192.168.56.102	9100	9110

You can use the same file with all the IPs in all the machines.

4. Modify the launch file robot_template.launch in patrolling_sim/launch
for use your own robot. The file have to run all the robot node for use move_base
and amcl. The launchfile have to use an arg named robotname for the namespace.
Patrolling node sends necessary commands to robots through /robot_name/move_base action server, 
read the robot pose from the topic /robot_name/amcl_pose and provides the map with the 
map_server on the topic /map.

5. Start the script 'start_experiment_distributed.py' on all
the robots.


6. On each robot, select the map, the number of robots, the algorithm
and other info in the launcher. Choose the proper network tcp_interface
(e.g. wlan0) corresponding to the robot network.
The only differences among the different robots should be these ones:
* From-To: each robot has a unique identifier
* Monitor: true only on one robot.

*Note*: the monitor program is a centralized node that is used only to collect information about the robots and to compute the performance. It does not send any information to the robots (except for the start signal at the beginning), thus it is not a centralized component of the system.

