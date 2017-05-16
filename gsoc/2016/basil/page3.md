# Introduction

19 Jun 2016

# UPDATES

Its been two weeks since my last blog.Its mainly because I did not have much to write about anything.As I have discussed before, one of the chief Job is Implementing the Graphics View framework.Which was new for me.So most of the time I was spending learning about these. So far I have successfully created a UI for the tool. With all those docked windows for logs,tools… And I have also added the graphics View framework tools like QGraphicsView,QGraphicsScene,QGraphicsItem.

I created a custom made GraphicsItem for this.Before starting I was thinking about something simple and basic.But When I made it looked cool to me.Each item will act as a node in the tree.

I used a QGrpahicsView subclass instead of directly using it.It will make the development of future versions super easy..

Apart from these things I have added the context menus what appear when we right click on components and backgrounds.Attached some signals with its slots.Also have some events like wheel events and right click events. Things are going well now..

I believe that I can do all these basic things in 2-3 days.

The only big challenge before me during that will be making a new algorithm for drawing the tree.Because In the old version,The nodes were round in shape and all connections were from and to its centers.So I have to make a new algorithm for that as the new nodes are rectangle in shape and connects are not from its center but from a extended arrows from the node..

I have not sent any pull request even though I have pushed it into my repo.Since the tool is currently in unstable state,I think I just need to put a version in my github account for backup.

* * *

Basil M Varghese