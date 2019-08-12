# Integrating Knowrob

## Files of interest

* _patrolling.owl_ in src/stacks/knowrob/knowrob_actions/owl - This is the task ontology and can be viewed in the Protege editor or queried with prolog.
* _kobuki.owl_ in src/stacks/knowrob/knowrob_srdl/owl - This is the robot description and can be viewed in the Protege editor or queried with prolog.
* _query_patrolling.py_ in src/stacks/knowrob/json_prolog/examples - This is the file that interfaces between the owl files and normal ROS nodes. Currently it doesn't publish anything, but one can be added easily. An example query is shown.
* _patrolling_json_prolog.launch_ in src/stacks/knowrob/json_prolog/launch - This is the launch file that lets you run _query_patrolling.py_.
* _TaskService.srv_ in src/task_utilities/srv - This is the service file that is used to make task query requests and responses.


## Running

In order to run the python sript that queries knowrob, first run

		roslaunch json_prolog patrolling_json_prolog.launch

to start the json_prolog server. Then in another terminal window run

		rosrun json_prolog query_patrolling.py
        
to start the node that listens for a service call to /query_task. The service takes in a task name and robot ID as strings. Right now the robot name is irrelevant because we're only using the kobuki robot. The task should be some form of "patrolling". Capitalization doesn't matter. The server will return two string lists, positive\_capabilities and negative\_capabilities which list the capabilities the robot has classified as positive or negative according to the task. To test this, in another terminal window you can run

		rosservice call /query_task "patrolling" "robot1"

Right now the only positive capability is "speed" and the only negative capability is "mass". More can be added to the ontology files of the patrolling task and kobuki robot.



Examples of prolog queries to get useful data can be found [here](http://www.knowrob.org/doc/reasoning_about_actions).


