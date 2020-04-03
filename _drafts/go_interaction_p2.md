---
layout: post
title:  "Building Efficient Manipulation Interactions for 3DOF Virtual Reality Controllers Pt.2"
author: Talal Alothman
date:   2020-03-16 09:00:00 -0400
description: Developing 3DOF manipulation is hard. This post proposes an efficient, user friendly implementation as a solution.
image: 
---

## Transition

In part one, I discussed some of the challenges and solutions for building selection, translation, and rotation interactions for 3DOF controllers. In this post, I will further elaborate on scale interaction in addition to methods of transitioning between the different interaction modes.

## Scale

More formally defined as the task of changing the size of an object (1), this task is fundamental to the proper manipulation of 3D models.

Building on the touch-pad mapping techniques from the previous two implementations made sense. At the same time we wanted to implement a technique that user's could possibly be familiar with. Mapping x-axis or y-axis changes on the touch-pad wasn't a good affordance. Why would swiping up-down or left-right cause a change in volume. Pinch to zoom was another consideration but bimanual gestures on a trackpad only accessble to the hold hand's thumb made for akward interaction. Seeing as this task doesn't have metaphors in the real world, made it harder. Unlike translation and rotation, scaling isn't part of our daily lives.

We approached this problem by attempting to find metaphors in relation to what effect scaling an object has, on 3D models. Expansion, compression, spread, suppress were all words that defined what happens when you scale. We experimented with a number of axis-based mappings but none felt good. It occurred to us that volume, as in the volume of a rigid body, was what was changing. Though the semantics are different, this relates to acustic volume in which conceptions of more and less are shared. We began to look at existing interfaces that handle volume controls. Some devices have dedicated hardware buttons. Others have software based sliders. There wasn't much variation unless you included [these](https://uxdesign.cc/the-worst-volume-control-ui-in-the-world-60713dc86950) horribly designed ones. Another implementation that came to mind was the volume click wheel on those Ipods from the 2000s. It controlled volume and also handled UI navigation.

This implementation isn't too difficult. From the touchpad we get a 2D vector of the location of a touching finger. This represents the displacement on the x and y axes from the center of the touchpad. We then use the angular displacement from frame to frame and map that to some displacement in the scale of the selected object. As the user scrolls in a circular manner around the touchpad, the size of the object grows or shrinks. We used a clock metaphor where moving clock-wise increases the scale of the object and moving counter clock-wise decreases it. This worked well. The sense of growing an object or shrinking mapped well with the metaphor of increasing volume on a Ipod.

![](..\assets\gif\scale.gif#half alt="Scaling Gif")
*Scaling*

![](..\assets\gif\scale_mapping.gif#quarter alt="Scaling Mapping Gif")
*Scale Mapping*

## Manipulation Mode Cycling

All the manipulation implementations discussed above cannot exist as possible actions by the user at once. Recall that we have only one controller with one touchpad. Attempting to predict what the user is attempting to achieve using data collected from initial touches is hard, especially when there's overlap with the manipulation mappings. Recall that the fishing reel technique requires users to scroll along the y-axis of the controller to bring the selected model closer or further back. That same mapping, y-axis scrolling should also allow the user to rotate the model around it's x-axis as mapped in the rotation interaction. How does one allow for these capabilities to exist without overwhelming the user and allow the user to accurately act on their intentions? Constrain them.

Instead of having all these functions accessible immediately and at any point, force the user to choose between different manipulation modes. This limits their degree of freedom when using the touchpad but allows them to clearly identify which core manipulation they aim to use. To achieve this we mapping the touchpad's press functionality to cycle between the different manipulation modes. Feedback is given to the user by identifying which mode their in using representative icons. These icons function as our visual signifiers informing the user which manipulation state they're in.

![](..\assets\gif\mode_change.gif#quarter alt="Mode Cycling Gif")
*Manipulation Mode Cycling*

## Pointer Grab

You might have noticed that when dealing with one model, the implementations above give you a sense of control. As mentioned in the selection section, group manipulation was the goal. Manipulating the 3D models in groups also worked well as we group their transforms into a parent transform and applied the mappings from each manipulation technique above to the parent transform.

Nonetheless, users still felt that the workflow for manipulating a model relative to another was slow and clunky. Independent observations and manipulations regarding each model by itself was fine but when the goal was to position models relative to each other, the task became harder.

As a solution, we build on affordances from common WIMP interfaces. In many WIMP interfaces pointers are capable of dragging items from one location to another. The action usually involves holding the mouse button down when the pointer is above a movable object. These objects are usually highlighted to identify their interaction status. A user will then move the mouse dragging with it the item it was pointing at. Once the user lifts their finger off of the button, the object is released in that position and the pointer is free to move on its own again. Funny describing something we do all the time in formal steps.

We use these same steps to provide a drag mechanism. When our ray-cast pointer hovers over a 3D model, the model highlights identifying it's in a hover state. The user can then hold down the trigger button and move the pointer to another location in space. The model will follow the pointers end at a set radius. This radius is set initially when the pointer makes contact with the model. This allows for quick placement of models relative to one another and relative to a set radius around the user.

## Things to improve

The manipulation actions presented in the previous posts present one approach to facilitating user interaction in 3DOF headsets that use 3DOF controllers. While the limitations of 3DOF systems can be avoid by using newer standalone headsets that support 6DOF headsets and controllers, I believe the need to understand what limits 3DOF systems have and ways to overcome them still remain relevant topics in the 3DUI space.

Throughout the development process, constraints on user interaction remained the most useful tool in clearly building manipulation techniques that were comprehendible. Taking into consideration the number of tasks that users needed to be able to do, allowing for the ability to do them all at once brought users to a state of confusion. Constraining degrees of freedom on manipulation tasks allowed user to compartmentalize what could and could not be done in their current state. It provided them with a way to confidently and accurately apply manipulation changes and observe changes to their clearly selected objects. This was, in my head, clearly a good example of "less is more".

One thing to note is that the main interaction metaphor used in our implementation, ray-cast pointer, is clearly an extension of the pointer in WIMP interfaces. While the hope is that with more immersive environments, users will be introduced with more "natural" forms of interaction, the current state of immersive VR applications tend to build on user experience and the affordances from 2D WIMP interfaces. It's yet to be seen if hand or speech interfaces will replace pointer based ones in immersive environments but building on the long legacy of 2D based WIMP interfaces seems like a logical first step. I think this implementation does a good job at that while dealing with the limitations of 3DOF controllers.

Constraints not clear and need to be emphasized using feedback and signifiers.
Bounding box does not provide enough granular interaction.

## References

1: Joseph J. LaViola, Ernst Kruijff, Ryan P. McMahan, Doug A. Bowman, Ivan Poupyrev. 2017. 3D User Interfaces: Theory and Practice 2nd Edition. Addison Wesley Longman Publishing Co., Inc., USA.

2: Doug A. Bowman and Larry F. Hodges. 1997. An evaluation of techniques for grabbing and manipulating remote objects in immersive virtual environments. In Proceedings of the 1997 symposium on Interactive 3D graphics (I3D ’97). Association for Computing Machinery, New York, NY, USA, 35–ff. DOI:https://doi.org/10.1145/253284.253301