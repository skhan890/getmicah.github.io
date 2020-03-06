---
layout: post
title:  "Building Efficient Manipulation Interactions for 3DOF Virtual Reality Controllers Pt.1"
date:   2020-02-20 21:00:00 -0400
---

# User Adoption

A while back I worked with a team of folks to develop VR tools for university level, dental educators and students. We reduced the main needs for the users to core manipulation functions with scanned 3D models. That means translation, rotation, and scale. Our tool took loads of inspiration from great VR applications like Tilt Brush, Quill, Poly. We had a good implementation running on desktop VR headsets (Rift, Vive) but we hit a barrier when it came to growing user adoption. Educators were willing to invest in one or two desktop headsets but no one was willing to buy a hundred or so desktop VR headsets in addition to the hundred or so dedicated rigs to drive them.

To scale our tool to the classroom, more affordable, mobile headsets were the way to go. We had previously tried low-end phone based VR like Cardboard but performance on devices varied and for the most part, nausea ensued, especially when batteries ran low. Students assumed others had an edge over them because they had better phones or had made sure to bring their chargers in with them. For the most part, that was true.

One option was the Oculus Go which had come out some months prior. I had gotten one and used it for a bit but didn't believe it could be used for more than media consumption. It also came with a 3DOF controller that was a lot more limited when compared with the controllers on the Rift or Vive. Our current interaction techniques were out of the question as they heavily relied on having a 6DOF controller.

I sought inspiration from other Go apps. There was heavy usage of ray-cast pointer techniques which was common with 3DOF controllers but most of these applications left me wishing for more. I finally came across Virtual Virtual Reality (VVR) which I felt allowed me to accurately control objects around me with one controller. It also used ray-cast pointer as it's main interaction technique but added gravity to many of the objects in the space around you. This gave the impression of each of these objects having weight and requiring your controller to fight off gravity to be able to lift them. Another characteristic was limiting manipulation to a grab mechanic that allowed you to move the object around the user in a set radius. You were also allowed to change the radius by swiping on the Y axis of the touchpad while an object is grabbed. This was really powerful and allowed accurate placement of 3D objects in the space around me; one of the game's main mechanics. I had hope.

![](..\assets\gif\vvr.gif#half)
*Grab Mechanic in Virtual Virtual Reality*

# Design & Implementation

The 3DOF Go controller had one main trigger button, a back button, a reserved "Oculus" button, and a two-axis, clickable touchpad. That's all we had to play with. The main challenge for working with a 3DOF controller is the inability to directly reach out and interact with the environment. The controller is able to map to it's proper rotation in the real world but unlike 6DOF controllers, it's position is not tracked. This is like trying to reach out into the world but having your hand stuck in one place.

As mentioned above, a common approach to solving this issue is using the ray-cast pointer interaction technique. This technique's main objective is extending a controller's reach by casting a ray into space that interacts with objects and surfaces. The intersection of the ray and an interactable object is signified using a cursor. This provides the user with feedback regarding where exactly they're pointing to. Ray-cast pointing is common within a lot of exiting VR applications and many users recognize it's affordances that support it's usage, such as ending the pointer with a cursor similar to that in WIMP-based interfaces.

![](..\assets\img\go_interaction\oculus_go_controller.png#half)
*Oculus Go Controller Diagram*

## *Select*

Selection is fundamental to manipulation interactions. Identifying which item a user intends on manipulating is the initial step before the actual manipulation happens. This means a user needs to be able to identify a particular object or group of objects from within the entire available, interactable set.(1)

In this implementation, when the pointer intersects with an interactable 3D model, the model changes color to light green to signify it's selectability; a "on hover" state. The location at which the ray intersects with the model's bounding box is signaled by a round cursor object. To select an object, a user points towards it and clicks the main trigger button on the controller. The main feedback here was changing the 3D model's color to light blue. This identifies it is currently in our selected set.

![](..\assets\gif\hover.gif#half)
*On Hover Feedback*

Initially, our goal to support only single object manipulation but we found that with single object selection, our users' ability to modify their 3D models in relation to one another quickly deteriorated. Group selection was a must. Our final implementation was similar to holding CTRL/CMD and using a mouse and it's corresponding cursor to select multiple objects. As group selection constituted a large portion of how users interacted with 3D models, we chose to remove the need for a button combination for group selection and allowed for direct group selection using the trigger button.

![](..\assets\gif\select.gif#half)
*Selection*

## *Translate*

Think of translation as modifying an object's location in the space around you. We considered menu-based indirect manipulation as a solution. This would involve dragging some slider and modifying the exact coordinates of the 3D model. This approach isn't fast and requires a lot of work, especially with non-expert users who are looking to moving things around and not position them at exact locations in space. This approach would be useful for very fine-grain modifications after a user had already placed object in a general vicinity of where they wanted it to be, not as our main form of interaction with 3D models.

*animation for menu based modification

The two-axis touchpad was another option. Users are used to scrolling with their fingers. Whether it's on a laptop touchpad or on their phones. The affordances of swiping your finger on a surface and having a cursor or a page update based in the direction your finger swiped are many and we wanted to capitalize on that. The only problem was, in VR, what would swiping your finger map to? On laptops and mobile phones, you're mapping your finger's movement to some response on a 2D surface. In VR, you're looking into a 3D environment. Where would your finger move these 3D models?

Our solution was to base it relative to your perspective. This means moving your finger left-to-right would move the object along the x-axis of a parallel, flat plane between you and the object (left-to-right of your perspective). Moving your finger up and down on the y-axis of the touchpad would move it back and forward from your perspective.

![](..\assets\gif\translate.gif#half)
*Fishing Reel Translation*

We found that moving an object left and right wasn't really useful. Seeing as you're stuck in one position with a 3DOF headset, moving it left and right relative to one's head didn't add to one's ability to better observe the 3D models. On the other hand, bringing it closer and further away provided the ability to better observe 3D models up close or push them further away for a more macro perspective. This is similar to the VVR implementation and turns out to be a well established interaction metaphor called "Fishing Reel".(2)

## *Rotate*

The main objective of rotating a 3D model is observing it's structure from multiple perspectives. Remember those spatial reasoning tests where you were asked to imagine the other side of a geometric shape. You don't actually have to do that when you're able to rotate a model in 3D space. While those spatial reasoning tests where useful in evaluating how well you could imagine the other side of some geometry, our users needed to be able to observe these perspectives and evaluate their state in order to review procedures.

*spatial reasoning example vs rotation animation

Rotating around the 3 axes is usually the ideal in 3D visualization software, usually implemented through widget based or indirect manipulation techniques. Our 3DOF controller was clunky at best when attempting those approaches. By building on our translation implementation, we mapped the two-axis touchpad to rotate around world's x-axis and y-axis. We reversed the mapping by associating the x-axis on the touchpad with the world's y-axis and the y-axis on the touchpad to the world's x-axis.

*diagram of mapping

Some part of the implementation above worked really well. From the user's perspective, moving your thumb along the x-axis of the controller's touchpad maps well with the model's rotation around the world's y-axis. On the other hand, rotating around the world's x-axis didn't make sense. From the user's perspective, if the camera's local x-axis aligned with the world's axis, it was great. The millisecond those two stopped aligning, even for a bit, the manipulation stopped making sense. Moving your finger along the y-axis of the touchpad resulted in the model's rotation around, from the user's perspective, an arbitrary axis that made little sense to the user. This was fixed by finding the vector between the model's position in space and the user's head position. We normalize that and then find the cross product between the world's up vector resulting vector. This gives us a vector that's perpendicular to those two resulting in a vector that always points to the right of the displacement vector separating the model's position and the user's head. This works and provides for coherent rotation from the user's perspective.

*incorrect rotation mapping

A final addition to the rotation implementation was locking the rotation along a specific axis. This wasn't an idea I was confident about but rotating both axes at the same time felt convoluted. The solution, limit the user's rotation DOF even further to allow for more precise coherent rotations. To do this we measure where the user's thumb lands over 3 frames and find along which axis the displacement is greater. This becomes their locked axis of rotation and allows the user the ability to precisely rotate the object along that axis until their thumb leaves the touchpad and the interaction state returns to a default state.

![](..\assets\gif\rotate.gif#half)
*Rotation*

## References

1: (Joseph J. LaViola, Ernst Kruijff, Ryan P. McMahan, Doug A. Bowman, Ivan Poupyrev. 2017. 3D User Interfaces: Theory and Practice 2nd Edition. Addison Wesley Longman Publishing Co., Inc., USA.)

2: (Doug A. Bowman and Larry F. Hodges. 1997. An evaluation of techniques for grabbing and manipulating remote objects in immersive virtual environments. In Proceedings of the 1997 symposium on Interactive 3D graphics (I3D ’97). Association for Computing Machinery, New York, NY, USA, 35–ff. DOI:https://doi.org/10.1145/253284.253301)