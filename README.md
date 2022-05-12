# autonomous-vehicles Notes & Resources

As we already know that the connected vehicle platforms, autonomous vehicle technologies are evolving and improving in terms of interoperability, flexibility and reliability. DDS has been gaining traction for that purpose, and The major reason behind that is that there are now multiple applications running on different computing platforms instead of multiple distributed ECUs (electronic control units). Just to give an example, before that approach actually developers had to build their software right embedded into ECUs because there was no need for an update for those software. However now we need an extendable way of data flow and communication protocol. And the standard single databus is a solution for that which simplifies the system for that purpose. With this approach in other words, When the needs evolve, that architecture can expand to respond the change and fulfill the future requirements without replacing or manipulating the core standards, so that decrease fragility and increase adaptability.

One of the pioneer standards in this field is Autosar. AUTOSAR is an open software architecture standardized by the automotive industry. In the former version which is called Autosar Classic, it is specialized in the functional ECUs where software is deeply embedded into them. They assumed that these applications would not require any updates in their lifetime. However in today's environment the requirements are changing and evolving, and in the end they decided to upgrade the architecture of Autosar. They released a new version of the platform called Autosar Adaptive. It is kinda runtime standardization for adaptive applications, and the main focus here is autonomous driving. So now that new version includes a binding to DDS (Data distribution service) as well.

Technically, DDS is a middleware protocol and API standard for data-centric connectivity, and with this abstraction, this architecture enables interoperability. The major point here is that DDS provides multiple integrations now even with non-autosar systems such as ROS and custom cloud-based technologies. The DDS API layer and additional abstraction layer in ROS2 also enable us to use multiple DDS vendors rather than strictly depending on only one. Adoption of dds also allows the Autosar standard to meet the requirements of level 4 and level 5 autonomous cars. That might be significant for us as well in order to cover the industry and customer requirements. Just to mention that It also provides developer friendly features such as traceability, which helps tracing to the original source of line, and understanding why code behave different than expected and finding the root-cause of the problem.

In software development and systems designing, the main overhead is doing the same thing over and over. And this chronic problem is very common in robotic industry as well. Therefore in 2007 in Stanford Intelligence Lab. the researchers have come up with an idea of creating a general purpose robot operating system: ROS.

Ros is widely used in industry and many developers worldwide building several libraries for the platform, you can find packages for sensors, lidar, depth camera, algorithms, and navigation like vehicle knows where exactly it is in its environment. It has several tools for simulations (both 2d 3d), Logging, Testing, Debugging, and many more.

ROS is mainly a publish and subscribe framework. Primary mechanism of ROS is using nodes that exchange to each other messages being transmitted to a topic, each topic has a unique name for a specific purpose. Using this mechanism overall provides us non-blocking processes, like fire and forget style. Nodes are entities that use ROS to communicate with other nodes. Messages are Ros data types used when passing data to a topic. Topics is like a pool where nodes can either publish messages to a topic, and subscribe to a topic to receive messages. So if I have a node that wants to share the information, that has to publish its message/data to a topic. And if another node wants to receive that message, it has to subscribe to that same topic to receive the message publisher subscriber topology can be organised in a flexible way as well, like the diagram you see here, many nodes can subscribe to the same topic at the same time. For instance, a motion sensor can be a publisher to a topic called movement maybe, and if we want a node to do something whenever a motion is detected, we should subscribe that listening node to the same topic to react depending on the position of our vehicle. Another example, if we want our vehicle to move or accelerate, then we need to register our controller as a publisher to the velocity topic, let's say to send that command to the system.

ROS2 is the new version of ROS with architectural improvements. Big players are using ROS2 due to its reliability, and paradigm shift happening. Just to mention that ROS1 support will be finished, and only ROS2 will have version updates and releases. In ROS2 some has changed yes, but some stays the same such as Nodes, messages, publishers, subscribers, and also command line, graphical tools (like Gazebo 3d dynamic simulator) all still there, and also ROS2 is backwards compatible which means that it can use both ROS1 and ROS2 libraries. ROS1 uses TCP as the underlying transport, which is unsuitable for lossy networks such as wireless links. But With ROS2 relying on DDS, it uses UDP as its default transport, so that we can give control over the level of reliability a node can expect and act accordingly.

(Note: If a small loss of data is not the main problem you can use UDP. That's why UDP is used for real time audio where latency is bad but a small loss of data can be worked around. It is used for things like syslog or SNMP where you can risk to loose a few data. If you instead need a reliable data transport, i.e. no loss of data then TCP is better because it acknowledges all the received data and will retransmit lost packets. Thus TCP is used for the Web, for mail transport etc.)

ROS2 incorporated DDS standards, the main improvement in ROS2 actually is that, and DDS stands for Data Distribution Service. DDS is an end-to-end networking middleware that simplifies complex network programming. It comes with some benefits such as a well documented software API, recommended use cases, less code to maintain, etc. DDS has been adopted for connectivity in next-gen autonomous applications by standard bodies in Automotive as well like AUTOSAR

ROS2 relies on DDS (Data Distribution Service), Real-Time Publish-Subscribe (RTPS) protocol, which allows different implementations of the standard to interoperate at this level. DDS has been adopted for connectivity in next-gen autonomous applications by standard bodies in Automotive as well like AUTOSAR. And That specification defines the wire protocol using a Model Driven Architecture in terms of a Platform-Independent Model. Middleware Interface is the significant component here. We have true abstraction by implementing multi layers such as DDS API and ROS client API. That abstraction enables us to safely switch DDS providers if needed without any modification. In terms of software architectural principles, this refers to decreasing the dependencies and increasing the flexibility and standardisation.

In order to make DDS an implementation detail, ROS 2 should preserve the ROS 1 like message definitions, format, and in-memory representation. So the ROS 1 .msg files would continue to be used, and the .msg files are converted into a compatible format to be used with the DDS transport. Language specific files would be generated for both the files as well as conversion functions for converting between ROS and DDS in-memory instances. The ROS 2 API works exclusively with the message objects in memory, and before publishing they are converted actually. Extensible and Dynamic Topic Types for DDS" (DDS-XTypes) is a specification that  allows systems to define data types in a more flexible way, and to evolve data types over time without giving up interoperability.

Composition uses executors to execute one or more nodes in a thread (in the case of SingleThreadedExecutor) or in multiple threads (in the case of MultiThreadedExecutor). Single and multi threaded executor are built in actually, but we can create our own if we would like to follow different behaviour in the creation of threads, or scheduling of callbacks, or the distribution of callbacks to certain threads, etc. Best practice would be to put all of our "business logic" for our node, in one set of source files, and implement it as a sub class of rclcpp::Node

* * *

Resources

https://www.apex.ai/post/porting-algorithms-from-ros-1-to-ros-2

https://index.ros.org/doc/ros2/ROSCon-Content/#roscon

http://docs.ros2.org/foxy/developer_overview.html#ros-middleware-implementations
https://design.ros2.org/articles/ros_on_dds.html
https://index.ros.org/doc/ros2/Installation/DDS-Implementations/
https://www.infoq.com/articles/ros2-dds-communication/

https://github.com/fkromer/awesome-ros2
https://fkromer.github.io/awesome-ros2/

https://gribot.org/preparing-ros-for-can-operation/
https://gribot.org/simulating-with-gazebo-and-ros/
https://github.com/gribot-robotics/documentation/wiki/Preparing-ROS-for-CAN-operation

https://autowarefoundation.gitlab.io/autoware.auto/AutowareAuto/

https://index.ros.org/doc/ros2/Releases/Release-Foxy-Fitzroy/
https://discourse.ros.org/t/shape-the-future-of-ros-2-documentation/12554/5

https://ubuntu.com/blog/exploring-ros-2-with-kubernetes
https://hub.docker.com/r/osrf/ros2/

https://vimeo.com/showcase/7812155/video/480450941

https://www.lgsvlsimulator.com/docs/create-ros2-ad-stack/
https://www.lgsvlsimulator.com/docs/autoware-auto-instructions/

https://www.mathworks.com/company/newsletters/articles/developing-longitudinal-controls-for-a-self-driving-taxi.html

https://www.rti.com/resources#resource_types=Whitepapers&resource_industries=Automotive&sort_order=Most%2520Recent

https://fast-dds.docs.eprosima.com/en/latest/fastdds/ros2/discovery_server/ros2_discovery_server.html

https://gitlab.com/autowarefoundation/autoware.ai/autoware/-/wikis/Gazebo-Simulation-Start

https://www.autoware.org/post/autonomous-valet-parking-2020
https://avp-project.uk/publications

https://discourse.ros.org/t/new-discovery-server/17383/5

https://ubuntu.com/blog/exploring-ros-2-with-kubernetes
https://ubuntu.com/blog/ros-2-on-kubernetes-a-simple-talker-and-listener-setup

https://semiengineering.com/getting-a-complete-picture-of-automotive-software/

https://github.com/ApolloAuto/apollo/blob/master/docs/howto/how_to_understand_architecture_and_workflow.md

https://www.96boards.org/blog/cyclonedds_on_kubernetes/
https://www.96boards.org/blog/cyclonedds_1/

https://github.com/ApolloAuto/apollo/blob/master/docs/quickstart/apollo_2_0_quick_start.md
https://github.com/ApolloAuto/apollo-platform/tree/master/ros/docs/design
https://apollo.auto/docment/Safety_First_for_Automated_Driving_handover_to_PR.pdf
https://github.com/ApolloAuto/apollo-platform

https://openadx.eclipse.org/

https://www.theconstructsim.com/ros-5-mins-045-rosbag-record-playback-ros-topics/

https://www.theconstructsim.com/robotigniteacademy_learnros/ros-courses-library/ros-autonomous-vehicles-101/

https://www.automotivelinux.org/
http://zenoh.io/

https://zephyrproject.org/
https://zephyrproject.org/micro-ros-a-member-of-the-zephyr-project-and-integrated-into-the-zephyr-build-system-as-a-module/
https://projects.eclipse.org/projects/iot.cyclonedds

https://www.simula.no/education/studies/software-tools-development-self-driving-cars
https://www.researchgate.net/publication/326562108_Tools_and_Methodologies_for_Autonomous_Driving_Systems
https://medium.com/@olley_io/what-software-do-autonomous-vehicle-engineers-use-part-1-2-275631071199

https://wiki.unece.org/pages/viewpage.action?pageId=87622236


https://developer.nvidia.com/blog/deep-learning-self-driving-cars/
https://github.com/udacity/self-driving-car
https://subscription.packtpub.com/book/hardware_and_creative/9781783554713/10/ch10lvl1sec87/introducing-the-udacity-open-source-self-driving-car-project

https://developer.nvidia.com/drive
https://www.nvidia.com/en-us/self-driving-cars/drive-platform/software/

https://en.wikipedia.org/wiki/Drive_by_wire

https://discourse.ros.org/t/ros-2-and-real-time/8796
https://www.rti.com/techtalk/integrating-dds-into-the-autosar-adaptive-platform

https://subscription.packtpub.com/book/iot_and_hardware/9781838649326/10/ch10lvl1sec96/interfacing-a-dbw-car-with-ros
https://learning.oreilly.com/library/view/ros-robotics-projects/9781783554713/ch10s17.html

https://forum.uavcan.org/t/an-exploratory-study-uavcan-as-a-middleware-for-ros/872
https://www.omg.org/spec/DDS-XRCE/About-DDS-XRCE/

https://www.youtube.com/watch?v=jbimBoI42AM

https://www.theconstructsim.com/start-self-driving-cars-using-ros/

https://design.ros2.org/articles/ros_middleware_interface.html

https://www.silexica.com/blog/ros-automotive-software-development/

Use the reference graphs in this tutorial: https://www.youtube.com/watch?v=jbimBoI42AM&feature=emb_logo

Driving safety: https://www.rand.org/content/dam/rand/pubs/research_reports/RR1400/RR1478/RAND_RR1478.pdf

https://aliasrobotics.com/files/realtimesecurity.pdf

AWS ADAS Data Lake: https://aws.amazon.com/blogs/architecture/field-notes-building-an-autonomous-driving-and-adas-data-lake-on-aws/

Autonomous Vehicle and ADAS development on AWS Part 1: Achieving Scale

Udacity Simulator Tutorial

Tutorial: End-to-end video tutorial of LGSVL simulator with Apollo driving

Autoware Course Lecture 14: HD Maps
https://uavcan.org/

Field Notes: Implementing Hardware-in-the-Loop for Autonomous Driving Development on AWS

https://ieeexplore.ieee.org/document/9233814

https://github.com/udacity/self-driving-car-sim
https://semiengineering.com/getting-a-complete-picture-of-automotive-software/
Webinars https://www.cargroup.org/
https://www.autosar.org/standards/adaptive-platform
https://www.researchgate.net/publication/340059734_A_Self-Driving_Car_Architecture_in_ROS2

https://ubuntu.com/blog/exploring-ros-2-with-kubernetes
http://gazebosim.org/tutorials?tut=ros2_overview

http://wiki.ros.org/socketcan_bridge

https://discourse.ros.org/t/project-aslan-open-source-announcement/15163
https://answers.ros.org/question/366163/send-a-can-frame-from-a-publisher-using-usb-to-can/

https://github.com/ROS4SPACE/ros2can_bridge
https://github.com/PickNikRobotics/ros_control_boilerplate
https://www.rosroboticslearning.com/ros-control
http://wiki.ros.org/cob_generic_can

How Adaptive AUTOSAR Enables Autonomous Driving
https://www.theconstructsim.com/category/ros-tutorials/

https://www.cnx-software.com/2019/02/07/autoware-open-source-software-autonomous-driving/

https://bitbucket.org/DataspeedInc/dbw_mkz_ros/src/master/

https://discourse.ros.org/t/hardware-requirements-discussion/12175
Similar Tools
https://www.whatnextglobal.com/post/autoware-open-source-self-driving-coming-of-age
https://news.voyage.auto/a-safety-first-os-for-voyages-driverless-technology-f87da4c363d3

https://medium.com/tri-ad-techblog/introducing-arene-c0c3c887b52c
https://www.tri-ad.global/areas-of-focus/arene

https://www.renovo.auto/

https://voyage.auto/technology/ (uses Renovo)
http://roadstar.ai/ (uses Renovo)

https://tier4.jp/en/

https://comma.ai/
https://www.apex.ai/

https://www.streetdrone.com/

https://web.auto/business/

DATA

https://data.tier4.jp/index/
