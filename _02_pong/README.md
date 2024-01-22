# Microgolf 02: Pong Ball

[![License: GPL v3](https://img.shields.io/badge/License-GPLv3-blue.svg)](https://www.gnu.org/licenses/gpl-3.0)

## Files

* <code>index.html</code> - Entry file for every web app
* <code>index.js</code> - JavaScript module for setting up an app
* <code>fullscreenCanvas.js</code> - JavaScript module for managing a full-screen canvas that self adjusts with browser window resizing
* <code>lib.js</code> - helper functions in one library

## Lesson notes

### 01 - Pong

![Pong](https://upload.wikimedia.org/wikipedia/commons/6/62/Pong_Game_Test2.gif)<br><sup>From [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Pong_Game_Test2.gif) under the [Creative Commons Attribution 3.0 Unported](https://creativecommons.org/licenses/by/3.0/deed.en) license</sup>

1. Designed and programmed by Allan Alcorn in 1972, _Pong_ is often considered THE game that launced the video game industry. The simulated physics system is certainly not realistic, but it's good enough to have been reused frequently because of its efficiency and simplicity
2. Review animation as repositioning between each drawn frame. The ball's {x,y} center changes every frame by some amount on the x axis and some amount on the y axis. These axis velocities can be negative values
3. Point out that the ball is a circle with a radius and the "walls" of the screen are lines defined by {x,y} coordinates. Consider how the collision detectin in _Pong_ could work

### 02 - Lerp and libraries

1. For good extensibility, this Pong-ball demo should work for balls of random radii, speeds, and headings. That way, we can test how "robust" the system is
2. For the purposes of this demo, there will be a minimum ball radius and a maximum ball radius as well as a minimum ball speed and a maximum ball speed. The actual radius and speed will be determined randomly within the min-max range. Consider a value between min and max as a percentage: zero percent is the min value and 100 percent is the max value. 50% would be exactly between the two. Normalized percentages are numbers between zero and one; divide the percentage by 100 so 50% becomes 0.5, for example. Consider using a number line as a visual
3. Linear interpolation ("lerp") is a simple algorithm that, given a min, max, and a normalized value, can find exactly the correct value in that range--or beyond if the normalized value is less than zero or greater than one
4. <code>a + (b - a) * t</code>, where <code>a</code> is the minimum, <code>b</code> is the maximum, and <code>t</code> is the normalized value. The range is maximum minus minimum (<code>b - a</code>) and the range translates along the number line by the minimum
5. Create a new file, <code>lib.js</code>, and export the lerp function. This could be useful in many projects, so we'll keep it in a separate module
6. In <code>index.js</code>, import the <code>lerp</code> function from <code>lib.js</code>

### 03 - Ball state and life

1. In <code>index.js</code>, new section commented as "ball state" that contains the <code>ball</code> object and the <code>resetBall</code> function
2. In the "init" section, <code>init</code> is a new function that calls <code>resetBall()</code> before starting the animation loop. This means <code>ball</code> will be populated with properties right away
3. In <code>resetBall()</code>, the <code>ball</code> object's position is set to the center of the canvas, and <code>lerp()</code> is used to define an angle (in radians) and speed. The polar-to-cartesian algorithm is used to define axis velocities. (Review of Euler integration might be necessary.) <code>lerp()</code> is then used to define a radius, then color and life are defined
3. The <code>ball.life</code> property should be short for now (e.g., 100 frames) because there's no collision detection; the ball will fly off canvas right away

### 04 - Update and draw ball

1. Delete testing code from <code>loop()</code> and add a new section commented as "update"
2. Point out the life countdown (number of frames) before <code>resetBall()</code>
3. Review Euler integration, if necessary. The ball's position is changed every frame according to its axis velocities. Don't add the collision detection until the next section
4. Draw the ball
5. Run the animation. Balls will fly beyond the edges of the canvas, but that's okay for now since <code>resetBall()</code> is called often enough

### 05 - Wall collision

1. Consider the ball's position relative to canvas width and height, specifically the ball's center point and radius relative to the <code>{x,y,w,h}</code> properties of the <code>canvas</code> object
2. If the ball is off the canvas even a little bit, change its position back onto the canvas and change its axis velocity by reversing its sign
3. Increase life (code uses 250 frames)
4. Run the animation. Balls will bounce off the canvas walls in a _Pong_-like manner