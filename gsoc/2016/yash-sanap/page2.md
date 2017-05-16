# week 2 updates

29 May 2016

# conceptualization

I have spent most of my initial time reading about different discovery techniques and implementations. Of all the implementations i have read about ice grid gave a lot of new ideas about the different features that can be implemented in to rcmaster. Also while reading about the Robocomp IDSL, I had a hard time figuring out what are the features of slice which is implemented in IDSL. So for helping other guys looking for the same I wrote an [tutorial](https://github.com/yashsanap/robocomp/blob/master/doc/IDSL.md) about IDSL.

So far I have compiled a list of features that can be implemented in to rcmaster. and also have been experimenting with the robocomp components.

*   **Component registration & lookup** - This is the basic feature of rcmaster. The components can register with the rcmaster and they will be assigned a port. Also any component can query about any other component based of number of criterias.
*   **Component deployment/monitoring** - RCmaster can also launch a set of components at start which can be set in the configuration file.Also when a query comes for a component the rcmaster could launch it if its not running. Also if a component is exits unexpectedly then rcmaster could take some specific actions like restarting that component or notifying any other component etc
*   **Load Balancing/Backup** - RCmaster may allow multiple replica of components for load balancing or backup. It could analyze the load of a component and can distribute the could resolve the future proxies to an low load component
*   **Gui For Master** - This can be achieved by modifying rcmanager to use rcmaster (can remove the tedious config file for rc-manger).
*   **Multiple masetrs** - This is one feature in which some effort and thought is required. we can keep them as backup and keep them in sync, may help in cases of temporary connection loses as it can handle local lookups in local master a master’s can communicate with each other to get remote name lookups. more discussion id needed on this.

Also i am thinking if we need to allow multiple masters in the same machine. This may act as backup or could share the query load

* * *

Yash Sanap