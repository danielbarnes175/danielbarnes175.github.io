---
layout: project
permalink: /portfolio/FacialTrackingCamera
---
# Face Tracking Camera

Checkout a video showcasing this project [here](https://www.youtube.com/watch?v=eH3_yiz7nn4)

A camera that can detect a person's face, and then rotate on servos to follow the person's face as they move around. This project was created with a team for HackISU X in 2018.

<svg class="svg-icon"><use xlink:href="{{ '/assets/minima-social-icons.svg#github' | relative_url }}"></use></svg> [View Source](https://github.com/danielbarnes175/HackISU2018) 

### The Three Stages

There were three stages in the success of this project:

1. Setting up the JavaFX frame to display the camera.  
2. Setting up OpenCV to detect faces.  
3. Setting up the motors and passing the input to correctly follow a user's face.

#### JavaFX

The UI part of this project is rather simple. We just wanted a way to see what our camera was seeing. We created a JavaFX frame, and connected the camera output to display in this frame.

#### OpenCV

The OpenCV part is the fun part. We used the haarcascade strategy for detecting faces. This looks for 2 out of 3 of eyes, mouth, and nose bridge. When there are two of the three, we can draw a rectangle around the face. This will contain the coordinates of the top left corner of the face, as well as the width and height. With this, we can figure out where our face is relative to our camera's "vision" and move the camera based on that. More on this in the next section.

#### Moving the Camera

So we know where our face is, right? Now, we just want to take those coordinates, and move them. We cannot move the face itself, so we must move the camera. Rather than shift the object on the graph, we are shifting the graph itself.

We serialized our rectangle data, and streamed it to an arduino. From the arduino, we were able to run C code that would drive the motors based on where we needed to move the camera. For example, if our user was to the left of center, we simply need to move the camera to look more left. Keep moving left until the face is within the bounds that we designated for the center of the screen.

We used two motors. One was for driving horizontal movement, and one was for driving vertical movement.

---

### What could be next?

Unfortunately, we didn't continue developing this project after the hackathon, but there are a lot of different features that could be added. What we've created could be the base for some very fun stuff!

* Live Video Stream - Setup the camera video output feed to stream to a website  
* Voice Control - Use the Amazon Alexa API to implement voice control
* Audio - Have audio on the device to speak based on what a user is doing  
* Sentry Mode - Rotate the camera looking for faces, and when it finds one, start tracking  
* 360* movement - Give the camera the ability to rotate 360 degrees  
* Independence from laptop - Switch the program to run off a Raspberry Pi, so it can be more mobile  
