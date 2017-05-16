# week 3 updates

05 Jun 2016

# Basic coding

Now that i am decided about the different features about the rcmaster. Pablo suggested about using something similer to how ros 2.0 is implementing node discovery. They are planning to use an external middle-ware named DDS. DDS uses an custom lightweight discovery protocol for finding other nodes. It eliminates the need for an central node keeping registry of all nodes. But after some discussion with other community members we decided to stick with rcmaster as using DDS will negate robocomp’s middle-ware independent policy and also it doesn’t give much benefits compared to the complexity in implementation.

Also we discussed about different ways to implement the multi robot scenario. Multi Robot scenarios can be implemented in 2 ways. one, we could have only one robot running rcmaster and all other robots will be configured to use this rcmaster. in This case the whole load will be into this rcmaster. This doesn’t require any extra coding in the rcmaster. we will only need to change the environment variable which points to the rcmaster in all robots. But the downside is there may be heavy latency as there is only one rcmaster and all the lookups and registrations are RPC’s. Other downside is that if that one Robot which is hosting the rcmaster fails or gets disconnected, then all other robots will fail. The 2nd solution is to let all robots have local rcmasters and they will need to sync their registry with all other robots in th network. But in this case the issue is how will the robots find each other. One straight forward solution is to hard-code all the robots ip in each of the robots. But this may be really tedious. Hence we may use some discovery protocol like udp multicast for finding other rcmasters.

Also this week i wrote a basic interface for the rcmaster. Both the idsl and and the slice file. and had a discussion about them with community members. After making a few changes suggested. I will begin with rest of implementation.

* * *

Yash Sanap