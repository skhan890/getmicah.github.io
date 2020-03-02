---
layout: post
title:  "Mapping Canonical Manipulation Tasks to a 3DOF Virtual Reality Controller"
date:   2020-02-20 21:00:00 -0400
---

## Scale

You'd need to be able to scale objects in the world around you. You do it everyday to books, screens, a lost dollar on the ground. In those cases you're bring objects closer to your eyes. This makes the details on those objects clearer allowing for a closer inspection. This common behavior is also very common in digital design and dentistry. More formally defined as the task of changing the size of an object (1), this task is fundamental to the proper manipulation and comprehension of 3D models.

For the scale task building on the touch-pad mapping techniques from the previous two implementations made sense. At the same time we wanted to implement a technique that user's could possibly already have a grasp on. Mapping x-axis or y-axis changes on the touch-pad wasn't a good affordance. Think about it, why would swiping up-down or left-right cause a change in volume. Seeing as this task doesn't have metaphors in the real world, made it harder. Scaling isn't part of our daily lives. Most things we interact with on a day to day basis aren't scalable or would require us to remanufacture at a difference size or scale factor.

The way we approached this was by attempting to find metaphors in relation to what effect scaling object has on 3D models. Expansion, compression, spread, suppress were all words that defined what happens when you scale a 3D model up or down. We experimented with a number of axis-based mapping but none felt good. It occurred us that volume, as in volume of a rigid body, was also what was changing. What immediately came to mind was the volume click wheel on those Ipods from the 2000s. It controlled volume and also menu navigation but the connotations with volume were strong in my head.

This implementation isn't too difficult. From the touchpad we get a 2D vector of the location of a touching finger. This represents the displacement on the x and y axes from the center of the touchpad. We then use the angular displacement from frame to frame and map that to some displacement in the scale of the selected object. As the user scrolls in a circular manner around the touchpad, the size of the object grows or shrinks. We used a clock metaphor where moving clock-wise increases the scale of the object and moving counter clock-wise decreases it. This worked well. The sense of growing an object or shrinking mapped well with the metaphor of increasing volume on a Ipod.

## Manipulation Mode Selection

All the manipulation implementations discussed above cannot exist as possible actions by the user at once. Recall that we have only one controller with one touchpad. Attempting to predict what the user is attempting to achieve using data collected from initial touches is hard, especially when there's overlap with the manipulation mappings. Recall that the fishing reel technique requires users to scroll along the y-axis of the controller to bring the selected model closer or further back. That same mapping, y-axis scrolling should also allow the user to rotate the model around it's x-axis as mapped in the rotation interaction. How does one allow for these capabilities to exist without overwhelming the user and allow the user to accurately act on their intentions? Constrain them.

Instead of having all these functions accessible immediately and at any point, force the user to choose between different manipulation modes. This limits their degree of freedom when using the touchpad but allows them to clearly identify which core manipulation they aim to use. To achieve this we mapping the touchpad's press functionality to cycle between the different manipulation modes. Feedback is given to the user by identifying which mode their in using representative icons. These icons function as our visual signifiers informing the user which manipulation state they're in.

## Pointer Grab

You might have noticed that when dealing with one model, the implementations above give you a sense of control. As mentioned in the selection section, group manipulation was the goal. Manipulating the 3D models in groups also worked well as we group their transforms into a parent transform and applied the mappings from each manipulation technique above to the parent transform.

Nonetheless, users still felt that the workflow for manipulating a model relative to another was slow and clunky. Independent observations and manipulations regarding each model by itself was fine but when the goal was to position models relative to each other, the task became harder.

As a solution, we build on affordances from common WIMP interfaces. In many WIMP interfaces pointers are capable of dragging items from one location to another. The action usually involves holding the mouse button down when the pointer is above a movable object. These objects are usually highlighted to identify their interaction status. A user will then move the mouse dragging with it the item it was pointing at. Once the user lifts their finger off of the button, the object is released in that position and the pointer is free to move on its own again. Funny describing something we do all the time in formal steps.

We use these same steps to provide a drag mechanism. When our ray-cast pointer hovers over a 3D model, the model highlights identifying it's in a hover state. The user can then hold down the trigger button and move the pointer to another location in space. The model will follow the pointers end at a set radius. This radius is set initially when the pointer makes contact with the model. This allows for quick placement of models relative to one another and relative to a set radius around the user.

## Things to improve

The manipulation actions presented in the previous posts present one approach to facilitating user interaction in 3DOF headsets that use 3DOF controllers. While the limitations of 3DOF systems can be avoid by using newer standalone headsets that support 6DOF headsets and controllers, I believe the need to understand what limits 3DOF systems have and ways to overcome them still remain relevant topics in the 3DUI space.

Throughout the development process, constraints on user interaction remained the most useful tool in clearly building manipulation techniques that were comprehendible. Taking into consideration the number of tasks that users needed to be able to do, allowing for the ability to do them all at once brought users to a state of confusion. Constraining degrees of freedom on manipulation tasks allowed user to compartmentalize what could and could not be done in their current state. It provided them with a way to confidently and accurately apply manipulation changes and observe changes to their clearly selected objects. This was, in my head, clearly a good example of "less is more".

One thing to note is that the main interaction metaphor used in our implementation, ray-cast pointer, is clearly an extension of the pointer in WIMP interfaces. While the hope is that with more immersive environments, users will be introduced with more "natural" forms of interaction, the current state of immersive VR applications tend to build on user experience and the affordances from 2D WIMP interfaces. It's yet to be seen if hand or speech interfaces will replace pointer based ones in immersive environments but building on the long legacy of 2D based WIMP interfaces seems like a logical first step. I think this implementation does a good job at that while dealing with the limitations of 3DOF controllers.

## References

1: (Doug A. Bowman, Ernst Kruijff, Joseph J. LaViola, and Ivan Poupyrev. 2004. 3D User Interfaces: Theory and Practice. Addison Wesley Longman Publishing Co., Inc., USA.)

2: (Doug A. Bowman and Larry F. Hodges. 1997. An evaluation of techniques for grabbing and manipulating remote objects in immersive virtual environments. In Proceedings of the 1997 symposium on Interactive 3D graphics (I3D ’97). Association for Computing Machinery, New York, NY, USA, 35–ff. DOI:https://doi.org/10.1145/253284.253301)