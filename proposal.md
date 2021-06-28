Proposal: spring() timing function in CSS and Web Animations
=======================

We propose adding a new timing function for transitions and animations to simulate the effect of a spring-based motion between the endpoints.

Over the past few years, Apple's designers have moved to more physically-based motion effects. Not only do these look more natural and fun, they also are often easier to tweak to get a desired effect. By far the most common type of motion we've seen is a simulation of a spring.

There are a lot of online resources discussing why this is good. Here is one of my favourites: Creating Animations and Interactions with Physical Models --> https://iamralpht.github.io/physics/

We're still discussing what is the best way to expose springy animations. Some internal feedback has suggested that our parameterisation below is too hard to author, and that we should favor a more simple F = -kx approach. I'm sure there are lots of people reading who have opinions. Please share these opinions!

Here is what we propose today as a starting point:

spring(mass stiffness damping initialVelocity)

  Simulate a spring using the solving algorithm defined by this JavaScript function [1].

  mass: The mass of the object attached to the end of the spring. Must be greater than 0. Defaults to 1.

  stiffness: The spring stiffness coefficient. Must be greater than 0. Defaults to 100.

  damping: The damping coefficient. Must be greater than or equal to 0. Defaults to 10.

  initialVelocity: The initial velocity of the object attached to the spring. Defaults to 0, which represents an unmoving object. Negative values represent the object moving away from the spring attachment point, positive values represent the object moving towards the spring attachment point.

NOTE: The definition of spring() above uses spaces to separate parameters because "So I tied an onion to my belt, which was the style at the time" [2]. Since all the other timing functions are comma-separated, maybe it is better to be consistent?

What's unusual about this form of timing function is that the animation effect is now independent of its duration. The spring timing function completely controls how the animation reaches its end point, and certain parameter values can produce an animation that does not settle at the end point before the animation duration expires (technically they never completely settle).

We therefore also propose a new keyword for duration: "auto", which means the duration will be calculated to be the time where the animation has settled.

NOTE: Lots of hand-waving here at the moment. Firstly, what "settle" means to most people is dependent on the type of animation, and the size of the animating object, and the distance being animated over. Secondly, we'd need to describe how this works for a keyframed animation, where the duration applies over all the keyframes.

The spring() timing function as described above has been implemented in WebKit. It is currently exposed in the Safari Technology Preview, although note that the current implementation does not handle optional parameters (you have to specify them all). It's not exposed in regular Safari builds - we'll consider that if we can reach consensus here.

For what it's worth, the implementation in WebKit was fairly simple. We don't think this is a big burden to browsers.

Meanwhile, here is a demo page that has the effect implemented in JavaScript [3]

Of course, all this should apply to Web Animations... left as an exercise for the reader :)

[1] https://webkit.org/demos/spring/spring.js

[2] https://frinkiac.com/?q=style%20at%20the%20time

[3] https://webkit.org/demos/spring/

With lots of love,

weinig and dino