# Smart Monitoring and Algorithmically Routed Traffic [SMART] Algorithm #

Repository here, explains the working process of SMART’s algorithm and aims to minimize the time taken to clear a junction given a traffic scenario.
This Algorithm with conjunction with Computer Vision software like YOLOv3, detect number of cars in each lane thus complete smart traffic system.


## Assumptions ##

- At Least 3 outgoing lanes with 1 left lane, 1 center lane and, 1 left lane.
- Ideal scenario where car which remains in the lane corresponding to where they want to go. Eg, cars which want to go right remains in right lane.
- Max amount of time taken by a car to cross the junction is 3 secs.
- No pedestrian lane for the time being.
- Car moves in pipelined manner, time interval being 1 sec.

<p align="center"> 
<img src="https://github.com/rahools/SmartTrafficAlgorithm/blob/master/img/pipelineEdit.jpg">
</p>


## Working ##

Switching algorithm is based around **graph theory** and **pipeling concepts**. To understand algorithm, working is divided into many sub units, which are as follows


### Graph ###

We can think of the junction as a graph, where we only represent outgoing lanes and lanes which can switched green simultaneously are connected. Left lane can be excluded as it can independently switched green or red.

<p align="center"> 
<img src="https://github.com/rahools/SmartTrafficAlgorithm/blob/master/img/graphEdit.jpg">
</p>


### Properties of graph ###

- Each vertex / node is associated with a pattern, i.e., list of lanes that can be green simultaneously with nodal lane. 
- Each edge has a weight which is equal to the sum of cars in the connected nodes.
- Each node also has a weight, weight of a node tells us how urgent is to switch green a pattern including that node. Weight of node is calculated by getting the sum of weight of each connected edge or this can calculated directly by,

                        Nodal weight = 3 * Cars in Node + Cars in each connected node 
                       
### Traffic Light Switching ###

Node with max weight is selected to be switched green and pattern is chosen by selecting edge connected to selected node with max weight. This pattern is then switched green for a fixed time period, light can be switched back to red earlier if car in lane exhausts. This process is repeated forever in order to route the traffic. To avoid the scenario where a pattern is repeatedly switched green again and again, a pattern can only be switched once per loop.


### Switching Time ###

Now we have to decide time period for which a pattern gets switched to green. We can decide this by following ways,

- Max number of cars that can cross in a pattern. Eg, if we decide number of cars to be 8, then by pipelining concept we know that signal has to be green for 8 secs followed by at least 2 secs for yellow light.
- Average worst waiting time for a lane. Eg, if we decide avg worst waiting time to be 90 secs, max number pattern for 1 node = 3, therefore number of remaining patterns left = 9. Avg worst waiting time can be divided into these pattern, applying pipelining concept, signal can be green for 8 secs followed by 2 secs for yellow light.


## Performance comparison ##

In order to compare performance to traditional traffic system, similar algorithm was made for traditional system to calculate time taken to empty out the junction. We observe that, 

- In low traffic scenario, SMART takes 65 secs while traditional system takes 132 secs
- In high traffic scenario, SMART takes 212 secs while traditional system takes 264 secs

As we observe, as traffic increases, performance of SMART comes near traditional traffic system.

