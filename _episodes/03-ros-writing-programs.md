---
title: "Writing ROS Programs"
teaching: 5
exercises: 0
questions:
- "Question to be answered?"
objectives:
- "List the objectives here"
keypoints:
- "List the key points here."
keypoints:
- "List more key points?"
---

1.

###  Creating and building a ROS Package (compiling hello world)

A simple package has a structure which looks like this within the catkin workspace 'src' folder.

```
 my_package/
 	CMakeLists.txt
 	package.xml
```
#### 1. Create the package


```
$ cd ~/catkin_ws/src
$ catkin_create_pkg your_pkg_name std_msgs rospy roscpp
```
This will create a `your_pkg_name` folder which contains a package.xml and a CMakeLists.txt, which have been partially filled out with the information you gave catkin_create_pkg. The dependencies supported by the package are `std_msgs`, `rospy` and `roscpp`

#### 2. Write your first program (ROS Publisher)

Create a folder within your ros package to store your codes.

```
$ mkdir /scripts
```
 
```
 my_package/
 	scripts/
 		helloworld.py
 	CMakeLists.txt
 	package.xml
```

Conventionally, python codes are stores in a folder named `/scripts` and C++ codes are stored in a folder named `src` within the package. This naming conventions is not required, but generally suggested that you follow.

Use your favorite editor to create a python file named `helloworld.py`

```
$ sudo gedit /scripts/helloworld.py
```
Every Python code must have this declaration on top:
```
#!/usr/bin/env python
```
This ensures that your files excexutes as a python script.

First, we will import the ROS Python library. Then we will import the std_msgs/String message type that will allow us to publish strings.
```
import rospy
from std_msgs.msg import String
```
The other message types supported by std_msgs package can be found here.-> [std_msgs Msg/srv Docs](http://docs.ros.org/api/std_msgs/html/index-msg.html)

Now we define the ROS publisher:
```
pub = rospy.Publisher('topic_name', String, queue_size=10)
rospy.init_node('node_name')
```

The `rospy.Publisher` instance is the main line of code that your program will use to interact with the ROS system. This declares that your node is publishing to a defined topic with the defined message type. The `rospy.init_node` initializes the ROS Client Library and registers your program as a node with the ROS Master. 

`rospy.Rate` defines the speed at which a loop is executed. Here it signifies that the loop executes 10 times per second. 

```
r = rospy.Rate(10) # 10hz
while not rospy.is_shutdown():
	pub.publish("hello world")
	r.sleep()
```
`rospy.is_shutdown()` is a flag that checks if the program should exit. This allows you to quit the python program with Ctrl-C.

Finally, the `pub.publish` instance publishes the String message. 

```
#!/usr/bin/env python
import rospy
from std_msgs.msg import String

pub = rospy.Publisher('topic_name', String, queue_size=10)
rospy.init_node('node_name')
r = rospy.Rate(10) # 10hz
while not rospy.is_shutdown():
	pub.publish("hello world")
	r.sleep()
```

Save and exit your program and make it an executable. 

```
$ chmod +x helloworld.py
```

Now build your code. 

```
$ cd ~/catkin_ws
$ catkin_make
```


ROS Publishers and Subscribers in C++ and Python using simple Talker + Listener programs
Defining custom Messages
ROS Parameter Server
 Using RQT Plot + rosbag
