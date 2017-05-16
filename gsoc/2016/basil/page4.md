# Update

22 Jun 2016

# Update –Week 6

Recently I was coding some slot functions of what happens when we click on the actions like save,Open,exit.. The old code helped me lot in this case.. In between I tried to put a function to select what kind of context menu should be displayed on screen when we right click on QGraphicsScene..I mean the context menu which appears when we right click on node and its background should be different.I made a silly mistake during that and spend 2 hour going through documentations to understand my mistake :D :D..

Now I will be going into drawing the graphic Tree.Since the new nodes are not circular in shape,We can’t distribute the other connected nodes evenly around a node.So I will have to use a new algorithm for that..Basically I will have to use the same spring force system..But putting some limitations on the node connection directions..Once these drawing have finished,I have to enter the communication part..

I haven’t started coding any communication part yet..I had already learned some Basics about the Zeroc Ice for understanding the robocomp framework in the beginning.So it helps me to build the tool in complete independent modules for GUI,Communications and controlling components.

I have also decided to use subclasses of all three main classes in qt Graphics View network..ie now We are using the subclasses of QGraphicsView,QGraphicsScene and QGraphicsItem instead of using direct classes..This will help to add some extra functionalities to the tool in the future..

* * *

Basil M Varghese