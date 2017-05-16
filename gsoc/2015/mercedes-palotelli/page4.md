# _GSoC,_ Symbolic planning techniques for recognizing objects domestic

#4

, Inverse kinematics Graph Generator

13 Aug 2015

**ikGraphGenerator, an alternative to ik** : As the project has progressed, many improvements have emerged. One of them is the new component, `ikGraphGenerator`. This component has been developed by Professor Luis Manso, and its goal is to remove weight to the inverse kinematics in the process of handling objects with a robotic effector.

One of the problems of the inverse kinematics is that, given a target for a particular end-effector, it can calculate different solutions or values for each motor of the kinematic chain. For example, to pick up a cup, the IK can position the end effector at the target extending his arm more or less forcing or not the elbow or or separating more or less the shoulder. It doesn’t matter that the final position of the chain be more forced or more natural, the goal is reached and the solution is accepted.

This is not convenient, since there is no way to control the trajectory of the arm. The control on the trajectory arm is crucial at certain times, for example when the robot avoids collisions between his arm and another objects (tables, chairs, walls, including his own body). The current `inversekinematics` component does not allow us this control, so that we need the `ikGraphGenerator` component.

The main concept is to create a spatial graph where each node stores the pose of the point, [tx, ty, tz, rx, ry, rz], and the set of angular values for each motor of the kinematic chain (that has 7 DOFs). The links connecting the nodes whose positions are close to each other. In this way we can calculate paths between two different and separate points.

![Alt text](https://github.com/robocomp/robocomp-ursus/blob/master/components/ikGraphGenerator/etc/ikg.jpg?raw=true)

###What does the `ikGraphGenerator` component?

Basically the `ikGraphGenerator` realizes two functions:

1.  It calculates the graph with random poses and their respective angular values. To do this, first it defines a spatial cube that represents the workspace of the robot arm. In this space a set of poses are selected and are sent to the `inversekinematic` component as targets. The poses that are not achievable by the ik are automatically deleted. The resultant graph is stored in a file in order to use it in later calculations.
2.  Once the graph has been calculated and stored, we can send a target to the `ikGraphGenerator`. First the component searches the node `A` whose pose is closest to the initial end effector pose and the node `B` whose pose is closest to the target position (to do this, the `ikGraphGenerator` uses a fast low-dimension k-d tree). Then, the component calculates a path between node A and node B through the graph using Dijkstra’s algorithm, so that the arm moves through the graph from the position marked by node A to the position marked by the node B. Finally, in order to achieve the final target, the `inversekinematic` component is called to compute the final values of the joints, starting from the position marked by the node B.

![Alt text](https://github.com/robocomp/robocomp-ursus/blob/master/components/ikGraphGenerator/etc/GIK.png?raw=true)

In the next post I will describe how the whole system works with all the components, the `inversekinematic`, the `ikGraphGenerator` and the `visualik` component.

Bye!