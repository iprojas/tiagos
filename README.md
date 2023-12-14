WebVR Starter Kit: Make Your Own 💞Animated Planetarium
=================
In the [last example](https://glitch.com/~starter-aframe-fancy) we made our solar system look more realistic with textures. In this project we extend it further with animations. We also add some lighting to the sun. 

A-Frame is an easy way to build your own WebVR (Web Virtual Reality) scenes. You don't need to install anything and scenes can be made with simple HTML. Our starter kits have code that can be easily [remixed](https://glitch.com/help/remix/) to create their own scenes. 

This is the third of four projects in our [WebVR starter kit](https://glitch.com/@glitch/web-vr-starter-kit). The projects include:
* 1) 🌎[starter-aframe](https://glitch.com/~starter-aframe): Get started with your first A-Frame scene
* 2) ✨[starter-aframe-fancy](https://glitch.com/~starter-aframe-fancy): Add textures, models, and learn more about A-Frame's magic!
* 4) 🌌[starter-aframe-deep-space](https://glitch.com/~starter-aframe-deep-space): Create a totally different experience with custom planets


## 🚀 Exploring The Scene
This code contains the [A-Frame Orbit Controls](https://github.com/ngokevin/kframe/tree/master/components/orbit-controls) component. Drag your cursor or use your touchscreen to orbit around the scene. In most mobile devices you can pinch to zoom.


![Exploring the scene](https://cdn.glitch.com/88cd7191-3d8f-4f2d-81eb-6cd36c094184%2Forbit-controls-ani.gif?1541731532321)

## 🔑 Key Tool: The A-Frame Inspector
We recommend you use the [A-Frame inspector](https://aframe.io/docs/0.8.0/introduction/visual-inspector-and-dev-tools.html), a powerful tool that comes with A-Frame and allows you to explore and modify the code of your scene visually. Open it by going to your scene and pressing `<ctrl> + <alt> + i` on Windows or `<ctrl> + <option> + i` on Mac. 

![the A-Frame inspector](https://cdn.glitch.com/77ecffe2-9e20-4c26-b5d7-fd6c9e4a0d6e%2Faframe-inspector.png?1541721410592)

## 📓 Our Project/Code
* README.md - project info/instructions
* index.html - this is our scene, click "Show" to see it
* assets - a folder of goodies like models (3D shapes like asteroids) and images. [Check this out to learn how it works.](https://glitch.com/help/how-do-i/)

## 💻 What the code does
### 💞 The Animation Component
There are multiple ways to animate in A-Frame. 
A-Frame contains [animations](https://aframe.io/docs/0.8.0/core/animations.html) you can use with `<a-animation>`. Here is how we'd make mars rotate with it:


```
<a-sphere id="mars" position="0 0 12.5" radius=".4" src="#mars-texture">
  <a-animation attribute="rotation" from="0 0 0" to="0 360 0" dur="15000" repeat="indefinite" easing="linear"></a-animation>
</a-sphere>

```


But this starter uses [aframe-animation-component](https://www.npmjs.com/package/aframe-animation-component). It works with the [entity-component system](https://aframe.io/docs/0.8.0/introduction/entity-component-system.html)  we learned about in the last project and A-Frame developers say it's the future of animation in A-Frame. 

```
<a-sphere id="mars" position="0 0 12.5" radius=".4" src="#mars-texture" animation="property: rotation; loop: true; to: 0 360 0; dur: 15000; easing: linear;">
</a-sphere>

```

The animation itself has attributes. There [are many](https://github.com/ngokevin/kframe/tree/master/components/animation/) but the main ones we use here are:
* `property`: The type of animation you want such as rotation, scaling (making something large or small), color (changing color)
* `dur`: The **duration** of the animation. Smaller means faster!
* `from` and `to`: These are coordinates in the form (x, y, z)
* `loop`: Does the animation repeat? Is true than it does. 
* `easing`: The rate of change in an animation over time. See these [handy demos](https://easings.net/).

### 🔀 Mixins

With a planetarium you have two main animations:
* Rotation
* Orbit
We use these many times and it would be pretty tedious to add them all to each planet and moon. So we use [mixins](https://aframe.io/docs/0.8.0/core/mixins.html). These allow us to make a template and use it multiple times. For example here is a mixin that could be used to color anything red:

```
<a-mixin id="color-it-red" color="red"></a-mixin>
```

To use it all you need to do is use the `mixin` attribute with the name:
```
<a-entity mixin="color-it-red""></a-entity>

```
But what if you want something to be slightly different? You can always override it a bit like here we override the default duration of the animation to make the orbit a bit slower with `animation="dur: 90000;"`

```
<a-entity id="orbit-jupiter" mixin="orbit" animation="dur: 90000;">
  <a-sphere id="jupiter" position="0 0 24" radius="2.5" src="#jupiter-texture" mixin="rotation"></a-sphere>
</a-entity>
```

### 📦 Organize your animations with wrapping

As you see we also have wrapped the planet sphere in a new `<a-entity>`. That's because if you just apply the orbit to Mercury itself, you just make Mecury rotate. We need to create a container outside and make the container rotate. To visualize it we can add a geometry and color to the orbit and see what it looks like


![visualize orbit](https://cdn.glitch.com/88cd7191-3d8f-4f2d-81eb-6cd36c094184%2Fmercury-orbit.gif?1541731174838)

```
<a-entity id="orbit-mercury" mixin="orbit" animation="dur: 20000;" geometry="primitive:box; depth: 12; height: 12; width: 12" material="color: green; opacity: .5">
    <a-sphere id="mercury" position="0 0 6" radius=".4" src="#mercury-texture" mixin="orbit"></a-sphere>
</a-entity>
```

The Earth and Moon also has even more wrapping now because we need a container to make the orbit for the moon around the earth. 

### 🌗 Lighting
Adding [light](https://aframe.io/docs/0.8.0/components/light.html) is another way to bring your A-Frame scene to life. 

All A-Frame scenes have default lighting. When you add your own lighting, this default lighting goes away.

Here we make the sun into a light. So we can see the planets lit as they face the sun. This is `point` type of lighting. A catch is we add special shading of `material="shader: flat;"`  to the sun so that it's immune from the lighting, otherwise the sun itself would not be lit.

But we also add some `ambient` lighting which lights the whole scene. Try removing that and seeing what happens. 


![what the scene should look like without ambient light](https://cdn.glitch.com/88cd7191-3d8f-4f2d-81eb-6cd36c094184%2FA-Frame_Starter_Kit_-_Mini_Animated_Planetarium.png?1541731881475)


## 💫 Remix me!

[Remix](https://glitch.com/edit/#!/remix/starter-aframe-fancy) this to make your own planetarium. Some fun ideas:
* Change orbit speeds
* Change starting positions of planets
* Change the color and intensity of the sun's lighting
* Remove all lighting except for the sun's and see what it looks like
* Add planetary [shadows](https://aframe.io/docs/0.8.0/components/shadow.html) (note this may make the scene slow on some devices)
* Add moons to the other planets using this [list of solar system moons](https://solarsystem.nasa.gov/moons/overview/)

## 🌟 What's next?
Check out the next starter project in the series 🌌[starter-aframe-deep-space](https://glitch.com/~starter-aframe-deep-space) to make a your own custom planets!