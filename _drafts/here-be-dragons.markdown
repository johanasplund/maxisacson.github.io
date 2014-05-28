---
layout: post
title: "Here be dragons"
categories: math code
---
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

*"What is this* `x_new` *stuff about?"* you might ask yourself. Now it seems kind of arbitrary and superfluous, but we will make use of them later when we tell pygame to draw lines for us, so bear with me for a while. Note also that we included the `math` library at the top. To calculate the new position we make use of some trigonometry. Since this is a post about fractals I'm not going to go in depth about the quirks of sine and cosine, so if it is unfamiliar to you you might want to consider consulting your favourite search enginge or just go with the flow and trust me on this one.

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
    steps = [1, -1, -1, 1]
    pattern = steps[:]
    for i in range(niter):
        window.fill((0,0,0))
        t.move(dist)
        for s in steps:
            t.rotateBy(s * 90)
            t.move(dist)
            pygame.display.flip()

        t.setPos(x_start, y_start)
        t.setAngle(0)
        tmp_steps = []
        for p in pattern:
            tmp_steps = tmp_steps[:] + steps[:] + [p]

        tmp_steps = tmp_steps[:] + steps[:]
        steps = tmp_steps[:]
        dist = float(dist) / 3.
   
{% endhighlight %}

[pygame]: http://www.pygame.org
