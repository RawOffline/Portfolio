# Swift G4
[itch](https://yrgo-game-creator.itch.io/swiftg4) [youtube](https://www.youtube.com/watch?v=rTBost7tW_s)

```
Duration:   04/2024 - 06/2024 (8 weeks) — 5th Project
Engine:     Unreal Engine 5
Genre:      First-Person Parkour
Team:       4 Programmers, 4 Artists
```
Swift G4 is a First-Person Parkour game heavily inspired by [Mirror's Edge](https://store.steampowered.com/app/17410/Mirrors_Edge/). <br> <br> The difference here is: You're sent to different post-apocalyptic timelines that needs to be eradicated. The way you do so is by activating a machine that will explode after a few seconds. In turn, you have to outrun the explosion by parkouring _(Running, Jumping, Vaulting, Climbing, Sliding and Wall Running)_ through each timeline and keep your momentum going. <br> <br> My main role in Swift G4 was the **Level Designer**, so here I'll mostly talk about my process and the mentality behind my choices.

<table>
  <tr>
    <td width="50%"><img src="/Gifs/SwiftG4_2.gif" /></td>
    <td width="50%"><img src="/Gifs/SwiftG4_3.gif" /></td>
  </tr>
  <tr>
    <td width="50%"><img src="/Gifs/SwiftG4_4.gif" /></td>
    <td width="50%"><img src="/Gifs/SwiftG4_5.gif" /></td>
  </tr>
</table>

## Level Design

| Explanation | Example |
| --- | --- |
| I began by spending some time looking through references of buildings and paths, both unique and mundane. By putting images together into a moodboard, I had a lot of inspiration to work with. Then I started whiteboxing on Unreal Engine. <br> <br> By going through this process, it was much easier to show the artists what I had in mind when building each section, and that made our cooperation so much better. | <img src="/Images/SwiftG4_References.png" alt="References" width="2000" height="auto"> |
| The idea was to have the ability to take multiple paths, to allow the player to figure out their own preferred way of traversing. In this scenario we have two main paths. <br> <br> Blue (Easy) — This path has you running up some stairs then into a hallway. This path is easy but will take longer than the other one. <br> <br> Red (Hard) — This path is tricky. It requires you to do a few jumps, then climb up a ledge. By landing into the climbing animation, you're slowed down a bit. But if you manage to make a wall run and jump at a good moment, you're able to avoid climbing, which in turn allows you to keep your momentum. | <img src="/Images/SwiftG4_LD1.png" alt="References" width="2000" height="auto"> |
| Red — Continuing with the red path, the player is able to make their way around using high-ground, involving jumps and wall runs. If they drop down, they're able to continue taking the other paths. <br> <br> Blue / Green — If the player took the blue path, they have a variation of different ways to take here while we're introducing our slide/crouch system. | <img src="/Images/SwiftG4_LD2.png" alt="References" width="2000" height="auto"> |
| On the other side of the wall, we have even more variation! Red and Yellow beginning on high ground. <br> <br> Blue and Green continuing from below. | <img src="/Images/SwiftG4_LD3.png" alt="References" width="2000" height="auto"> |


This mentality was used throughout the majority of the Level Design and it lead to quite a fun process for me. I wanted the players to be able to choose what path and type of movement they want to focus on, while still making sure that every movement mechanic will have its own time to shine. <br> <br> The challenging part came during our playtests, I realized that I need to work on clarity. How do I make the intended paths more clear for the player... **Lights!**


| Explanation | Example |
| --- | --- |
| Indeed, using lights and colors was the answer. In the attached example, there's a hallway to take on the far left side, but the pillars are blocking the view of the player as they come running in. <br> <br> By using two neon signs, a _red_ one on the right and a _green_ one on the left that looks like arrows pointing to the direction of the hallway, it made things much clearer. | <img src="/Images/SwiftG4_LD4.png" alt="References" width="500" height="auto"> |
| This mentality then allowed me to build more creative and interesting scenes, such as this broken, dark hallway with flickering red lights leading the player to the exit. | <img src="/Images/SwiftG4_Lights.png" alt="References" width="500" height="auto"> |
| Another example of the mentality used above with the flickering lights showing the path. | <img src="/Gifs/SwiftG4_1.gif" alt="References" width="500" height="auto"> |
