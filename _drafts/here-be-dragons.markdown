---
layout: post
title: "Here be dragons"
categories: math code
---
This here is the dragon curve, or at least an iteration of it.

![src](/assets/here-be-dragons/img99.gif)

Sometimes it is also known as the paper folding curve.

Lets draw a dragon curve fractal using [Pygame][pygame].  

We need something that draws the fractal for us. Let's start by making a drawing automaton embedded in a class I choose to name `turtle` that will handle all drawing. It needs to know a few things about it self: `xinit` and `yinit` will be the starting position and `window` will be the pygame window we will draw to (more on that later).

{% highlight python %}
class turtle(object):
    def __init__(self, window, xinit, yinit):
        self.window = window
        self.x = xinit
        self.y = yinit
{% endhighlight %}

Ok, nice. But we need methods to move our turtle a certain distance. To do that we add a function called `move`. However we need to know *in what direction* we want our turtle to go. We can add an `angle`-variable to keep track of the direction. When we're on it let's also add methods to set and rotate the angle.
{% highlight python %}
import math

class turtle(object):
    def __init__(self, window, xinit, yinit):
        self.window = window
        self.x = xinit
        self.y = yinit

    def setAngle(self, angle):
        self.angle = angle

    def rotateBy(self, angle):
        self.angle = self.angle + math.radians(-angle)

    def move(self, dist):
        x_new = self.x + dist * math.cos(self.angle)
        y_new = self.y + dist * math.sin(self.angle)
        self.x = x_new
        self.y = y_new
{% endhighlight %}

*"What is this* `x_new` *stuff about?"* you might ask yourself. Now it seems kind of arbitrary and superfluous, but we will make use of them later when we tell pygame to draw lines for us, so bear with me for a while. Note also that we included the `math` library at the top. To calculate the new position we make use of some trigonometry. Since this is a post about fractals I'm not going to go in depth about the quirks of sine and cosine, so if it is unfamiliar to you you might want to consider consulting your favourite search enginge or just go with the flow and trust me on this one

 *"But Max,"* I hear you say, *"why do you flip the sign on the angle?*" Good question. Let's talk a bit about coordinate systems. Usually in mathematics we use a plain Cartesian coordinate system with the y-axis pointing upwards and the x-axis pointing rightwards. Alse, positive angles are measured going from the positive x-axis counter clockwise. But computer scientists are not like us. The industry standard for computer graphics has long been to flip the direction of the y-axis. Therefore the origin is in the upper-left corner and the direction of increasing y-coordinates is downwards. So to continue counting angles going counter clockwise as positive on our screen we have to add a layer of translation between us and the computer, and we do that by flipping the sign of the angle. However this is purely a matter of preference, you can keep the clockwise going angles if you'd like.

Now it's time for the fun stuff, let's make our turtle actually draw something. To to that we need to import pygame, so add

{% highlight python %}
import pygame
{% endhighlight %}

at the top of the document. We also need to modify our `move`-method.

{% highlight python %}
#... other code ...
    def move(self, dist):
        x_new = self.x + dist * math.cos(self.angle)
        y_new = self.y + dist * math.sin(self.angle)
        pygame.draw.line(self.window, (255, 255, 255), 
                        (self.x, self.y), (x_new, y_new), 1)
        self.x = x_new
        self.y = y_new
{% endhighlight %}

Ok, with that out of our way, let's write the function that draws for us:

{% highlight python %}
def dragon(x, y, dist, niter, window):
    pygame.init()
    window = pygame.display.set_mode((640, 480))
    t = turtle(window, x, y)
    angle = 90
    t.setAngle(angle)
    steps = [-1]
    for i in range(niter):
        window.fill((0,0,0))
        t.move(dist)
        for s in steps:
            t.rotateBy(s*90)
            t.move(dist)
            pygame.display.flip()

        t.setPos(x, y)
        angle += 45
        t.setAngle(angle)
        tmp_steps = [k*-1 for k in steps[::-1]]
        steps = steps[:] + [-1] + tmp_steps[:]
        dist = dist/math.sqrt(2)
        time.sleep(0.5)   
{% endhighlight %}

This is quite a lot, so let's go through it. The first few lines just initialized Pygame, creates a window to draw in and initializes a turtle to do the drawing for us. We set out starting angle to 90 degrees, it's arbitrary and you can choose whatever you like. The next line that reads

```python
steps = [-1]
```

is our *axiom*, the initial state of our L-system. Fortunately the L-system for the dragon curve is quite simple. All we do is move a distance, turn left or right 90 degrees, draw again, and so on. We will populate the `steps`-list with either positive or negative 1's in an ordered manner to keep track of which way to turn.

Next is the heart of the function, the `for`-loop. We begin by clearing the screen by painting it all black (`window.fill(...)`) and do the initial movement of our turtle. Then we iterate over our `steps`-list. This is where we draw the fractal. For each entry `s` in `steps` we rotate by `s * 90` degrees. In effect this means that we rotate clockwise when  `s == -1` and counter clockwise when `s == 1`. After this we draw some more and and update the display window with `pygame.display.flip()`. I'm updating the display in the inner loop to get a nice visualization of each step taken in drawing the fractal. If you'd like you can put the update call in the outer loop. This will also speed up the drawing a bit. Next we reset our turtle at the starting position, increment the angle by 45 degrees and telling our turtle to face that direction. The stuff here about the angle is purely for display purposes, try to remove it and see what happens :^).


[pygame]: http://www.pygame.org
