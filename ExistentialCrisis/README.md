# Existential Crisis

```
Duration:   2024 (4 weeks) — 4th Project
Engine:     Unreal Engine 5
Genre:      Experimental Walking Simulator
Team:       Solo Project
```

Existential Crisis is an experiment of camera angles, inspired by [Resident Evil](https://en.wikipedia.org/wiki/Resident_Evil) and [Silent Hill](https://en.wikipedia.org/wiki/Silent_Hill). <br> <br> You get to play as a mannequin who seems to awaken in a foreign location, and they have to find their way out, but things are a bit... odd.

---

| Static Camera | Example |
| --- | --- |
| By having cameras attached to invisible boxes, I was able to trigger a swap of camera when the character moves into a box. <br> <br> This allowed me to show the player what I wanted them to see. <br> <br> Simple, but effective — _KISS-Principle!_ | <img src="/Gifs/ExistentialCrisis_StaticC.gif" alt="References" width="400" height="auto"> |

---

| Moving Camera | Example |
| --- | --- |
| The gif shows the POV of a camera with a large collider that is attached to a spline. When it collides into the character, it begins moving the camera along the path of the spline, as if being pushed by the character. In turn, it will stop when the character stops, and continue when they continue. <br> <br> In this circumstance, the spline is moving in an upwards angle while gradually twisting. <br> <br> With some tinkering and blueprinting I was able to make the cameras traversal quite smooth. | <img src="/Gifs/ExistentialCrisis_1.gif" alt="References" width="400" height="auto"> |
| It even works nicely when moving away from the camera, as shown on the gif. | <img src="/Gifs/ExistentialCrisis_1R.gif" alt="References" width="400" height="auto"> |

---

| Security Camera | Example |
| --- | --- |
| This is a variation of the Static Camera. <br> <br> As long as the character is within range of the box, the camera will constantly point at them. <br> <br> By putting it quite high up on a wall, I was able to make it feel like a security camera. | <img src="/Gifs/ExistentialCrisis_SecurityC.gif" alt="References" width="400" height="auto"> |

_Take a shot every time I said camera._
