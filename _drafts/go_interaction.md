---
layout: post
title:  "Mapping 6DOF Controller Model Manipulation to a 3DOF Controller"
date:   2019-10-06 21:00:00 -0400
---

# The problem

A while back I worked for a company that focused on developing VR tools for educators and students. We reduced the main needs for our users to core manipulation functions with their 3D models. That means translation, rotation, and scale. Users needed to be able to move the 3D models around, rotate them as they pleased, and scale them to their heart's desire. Building the interaction techniques was not that hard as we took loads of inspiration from great VR applications like Tilt Brush, Quill, Poly. We had a good implementation running on desktop VR headsets (Rift, Vive) but we hit a barrier when it came to user adoption. Educators were willing invest in one or two desktop headsets but no one was willing to buy a hundred or so desktop VR headsets in addition to the hundred or so dedicated rigs to drive them for their students. We wondered why.

To scale our tool to the classroom, more affordable, mobile headsets was the way to go. Our customers agreed. We had previously tried low-end phone based VR like Cardboard but performance on devices varied and for the most part, nasuea ensued, especially when batteries ran low. Students assumed others had an edge over them because they had better phones or had made sure to bring their chargers in with them. For the most part, that was true.

One option was the Oculus Go which had come out about 8 months prior. I had gotten one and used it for a bit but didn't believe it could be used for more than media consumption. It also came with a 3DOF controller that was a lot more limited when compared with the controllers on the Rift or Vive. Our current interaction techniques were out of the question as they heavily relied on having a 6DOF controller.

I sought inspiration from other existing Go apps. There was heavy usage of ray-cast pointer interaction which was common in with 3DOF controllers but most of the applications left me wishing there was more I could do. I finally came across one application, Virtual Virtual Reality which I felt gave me the freedom to do what I wanted, all with one controller. It also used a ray-cast pointer as it's main interaction technique but added gravity to many of the objects in the space around you. This gave the impression of each of these object having weight and requiring your controller to fight off gravity to be able to lift them. Another characteristic was limiting manipulation to a grab mechanic that allowed you to move the object around the user in a set radius. You were also allowed to change the radius by swiping on the Y axis of the touchpad while an object is grabbed. This was really powerful and allowed accurate placement of 3D objects in the space around me. I had hope.

# Implementation

The 3DOF Go controller had one main trigger button, a back button, a reserved "Oculus" button, and a two-axis clickable touchpad. That's all we had to play with. The main challenge for working with a 3DOF controller was one's inability to directly reach out and interact with the environment. The controller is able to map to it's proper rotation in the real world but not it's actually position. This is like trying to reach out into the world but having your hand stuck in one place. A common approach to solving this issue is using the ray-cast pointer interaction technique. This technique's main objective is extending a controller's reach by casting a ray into space that interacts with objects and surfaces. This is a common method with 3DOF based applications. This method also works well for beginners as interacting with UI is similar to how users use the mouse on a typical WIMP GUI. Ray-cast pointer needed to be our main interaction technique.

## Selection
Selection is fundamental to any set of interactions. Identifying which item a user intends on manipulating is the initial action before the actual manipulation happens. This means a user needs to be able to identify a particular object or group of objects from within the entire set of available, interactable set.(1) Whilst thinking of the contraints mentioned above (3DOF controller) interaction techniques that fall under the grasping metaphor become virtually impossible to implement. You'd need a 6DOF controller to acurately handle that. It was out of the question. The focus remained within the realm of pointing metaphors, ones that extend the reach of a 3DOF controller to that beyond it's immediate physical location. Ray-cast pointing is common within a lot of exiting VR applications and many users have existing affordances that support it's usage such as ending the pointer with a cursor similar to that in WIMP-based interfaces.

Initially, our goal to support only single object manipulation but we found that with single object selection, our users' ability to modify their 3D models in relation to one another quickly detriorated. Group selection was a must. Our final implementation was similar to holding CTRL and using a mouse and it's corresponding cursor to select multiple objects. To select an object, user's needed to point towards it and press the main trigger button on the controller. The main feedback incorporated was a change of the 3D model's color to a light blue. This identified it was currently in our selected set.

## Translate
Think of translation as moving the object around in space. 3D space has three dimensions(x,y,z). Translating would mean modifying where an object is in that space. We considered menu-based direct manipulation as a solution. This involves dragging some slider and modifying the exact coordinates of the 3D model. This approach doesn't feel intuative and requires a lot of work, especially with non-expert users who are looking to moving things around and not positioning them at exact location in space. This approach would be useful for very fine-grain modifications after a user had already placed object in the general vicinity of where they wanted it to be, not as our main form of interacting with the 3D models.

The two-axis touchpad was another option. Users are used to scrolling with their fingers. Whether it's on a laptop touchpad on even on their phones. The affordance of swipping your finger on a surface and having a cursor or a page update based on the direction and your finger swiped is a common one. The only problem is relative to what? On laptops and mobile phones, you're mapping your finger's movement to some response on a 2D screen. In VR, you're looking into a 3D environment, what and where would your finger be doing?

Our solution was to base it relative to your perspective. This means moving your finger left-to-right would move the object along the X-axis of a flat plane between you and the object (left-to-right of your perspective). Moving your finger on the up and down the Y-axis of the touchpad would move it back and forward from your perspective. We found that moving object left and right wasn't really useful. Seeing as your stuck in one position with a 3DOF headsets, moving it left and right relative to one's head didn't add to the ability of user's to better observe the 3D models. On the other hand, bringing it closer and further away provided the ability to better observe 3D models upclose or push them further away for a more macro perspective.

## Rotate
The main objective of rotating a model is observing it's structure from multiple perspectives. Remember that spatial reasoning tests where you were asked to imagine the other side of a geometric shape. You don't actually have to do that when you're able to rotate a model in 3D space. While this spatial reasoning tests where useful in evaluating how well you could imagine the other side of geometry, our users needed to be able to observe these perspectives and evaluate their state in order to draw conclusions.

Rotating around the 3 axes is usually the ideal in 3D visualization software usually implemented through widget based or value manipulation interaction techniques. Our 3DOF controller was clunky at best when attempting those approaches. By building on our translation implementation, we mapped the two-axis touchpad to rotating around the model's local x-axis and around the world's y-axis.
## Scale

## Pointer Grab

## Things to improve

## References
1: (Doug A. Bowman, Ernst Kruijff, Joseph J. LaViola, and Ivan Poupyrev. 2004. 3D User Interfaces: Theory and Practice. Addison Wesley Longman Publishing Co., Inc., USA.)