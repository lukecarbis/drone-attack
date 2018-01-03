# Drone Attack

[Open](https://lukecarbis.github.io/drone-attack)

WASD to move around. Look at the drone to fire your laser at them.

## To-do

- [ ] Resolve memory leak
- [ ] Drones explode after x seconds of being hit by laser
- [ ] Scores
- [ ] Timer
- [ ] Start / Restart

## Debrief

This was a fun first project! I ran into some interesting problems along the way, but mostly things went pretty smoothly.

A lot of the fun for me on this project has been playing with lights and sound. When the laser is activated, it moves a red spot light on the target.

The positioning of the sounds add a lot to the scene, and are super easy in A-Frame – I just made each sound a child of the element they were emitting from. You'll notice as you walk close to the drones they become louder, and the same is true for the sparking sound, while the laser sound emits from the camera so it's always the same volume.

I ran into lots of trouble with the particle component (the sparks) – it wasn't playing nicely with the environment component. It took my a while, but I eventually tracked it down to [this bug](https://github.com/IdeaSpaceVR/aframe-particle-system-component/issues/26), which I resolved (at least for now) by removing fog from the environment.

The position of the laser was another difficult aspect. It took my a while to realise that if I matched the start point with the camera position, I would be looking directly down the line, and thus unable to see it!

I'm not quite happy with the single pixel width line. Of course, I could use a cylinder, but shapes like that are generated with a `width`, `height`, `depth`, `rotation`, and `position`, as opposed to my ideal case: `start`, `end`, and `diameter`.

Another problem is that the start and end positions can change while the laser line is visible (if the camera or drone moves). I could lock the laser to the camera by making it a child of the camera, but there would be now way of locking it at on the drone end (plus I would have to deal with converting the world position of the of the drone to the relative position of the line in the camera).

So, rather than do that, I opted for the more resource intensive method of reapplying the start and end position of the laser line on every `tick`. In hindsight, this is far from ideal, and the likely cause of memory crashes (especially on mobile).

I did experiment with a Curve component, which allowed my to create a curve at a start and end position, and draw a shape along that curve (I used a repeating cylinder). Unfortunately, working with this component in every single tick was far too slow.

What I'd like to try next is drawing the laser as a child of the target (so that it moves with the target), and if the camera moves, just turn the laser off until a new `click` event occurs.
