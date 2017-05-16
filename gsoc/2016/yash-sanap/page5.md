# final week updates

20 Aug 2016

# Final Evaluation Updates

In this post I am concluding the work done during my GSOC period. I will outline the basic work done, results achieved and how to use the tools I have developed. Also I will provide a demo of the work done.

**Work Completed**

I have developed and thoroughly tested RCMaster. Also I have changed the C++ and python components to use RCMaster. Also these changes are incorporated into the code generator so that even the future components will use RCMaster by default.

I will list down the features of RCMaster now:

*   All the components can register to RCMaster in which they will be given a free port
*   The queries can be filtered by various parameters. e.g: A component can get the list of all the interfaces of name “test” running on the host 192.168.0.21
*   RCMaster periodically saves the database so that even if the RCMaster crashes it can recover the data on restart and prevent the hassle of restarting the components.
*   RCMaster has a cache implemented so that if a component is crashed it will be assigned the same port on restart. So that all the previous connected components can continue the connection.
*   The components will now wait until the required interfaces are up and if any of the interfaces go down then they will suspend until they are up. This allows us to start the components in any order.
*   RCMaster is designed to produce meaningful exceptions.

**Usage**

Here I will describe how to generate a sample component and how to run it

First write the cdsl description of the component client1.cdsl


```
import "/robocomp/interfaces/IDSLs/RCMaster.idsl";
import "/robocomp/interfaces/IDSLs/Test.idsl";
import "/robocomp/interfaces/IDSLs/ASR.idsl";

Component client1{
	Communications{
    	requires rcmaster;
    	implements ASR,test;
	};
	language Python;
};

```


Now generate the component by running `robocompdsl client1.cdsl ./`

Now generate another component client2.cdsl by running `robocompdsl client2.cdsl ./`


```
import "/robocomp/interfaces/IDSLs/RCMaster.idsl";
import "/robocomp/interfaces/IDSLs/Test.idsl";
import "/robocomp/interfaces/IDSLs/ASR.idsl";

Component client2{
	Communications{
    	requires rcmaster,test;
	};
	language Python;
}; Now edit the client2 to use the test interface implemented by client1\. Open `src/client2.py` file from the client2's directory and change the `@@COMP NAME HERE@@` to client1.

```


Edit the client to `src/specificworker.py` in client 2 and add the below code in the compute function.


```
try:
    self.proxyData["test"]["proxy"].printmsg("hello from " + self.name)
 except Ice.SocketException
       self.waitForComp("test",True)

```


Now edit the printmsg() in `src/specificworker` to add the below line


```
print message

```



Now we can run all the components.

First start the RCMaster and then the components


```
src/rcmaster.py    This will start RCMaster.    Now start client2 , then you can see that the client 2 is waiting for client1

```


src/client2.py –Ice.Config=etc/config

Now start client1


```
    src/client1.py --Ice.Config=etc/config

```


Now you can see the client1 printing “Hello from Client2” periodically. Play around

### Demo video

You can see the video of the above demo [here](https://github.com/robocomp/website/blob/gh-pages/img/rcmaster_demo.mp4)

### Links to commits

[All Commits](https://github.com/robocomp/robocomp/commits?author=yashsanap)

[Pullrequest1](https://github.com/robocomp/robocomp/pull/78)

Pullrequest2

Pullrequest3

* * *

Regards,

Yash Sanap