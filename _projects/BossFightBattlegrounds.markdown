---
layout: project
permalink: /portfolio/BossFightBattlegrounds
---
<figure>
    <img class="responsive-image" src="../assets/img/projects/BFB/BFB_Boss.png" alt="The Boss"  width="30%" />
    <figcaption>The Boss - Modeled off our friend Adam</figcaption>
</figure>

<svg class="svg-icon"><use xlink:href="{{ '/assets/minima-social-icons.svg#github' | relative_url }}"></use></svg> [View Source](https://github.com/danielbarnes175/BossFightBattlegrounds) 

# Boss Fight Battlegrounds

Now this one was a fun one. I worked on it with a team of 3 other students for Com S 309, or Software Development Practices, in Fall 2019. We ended up being voted the #1 best project out of 78 projects. We worked hard on this project, and it paid off when we had a working game at the end of the semester.

The game was developed almost entirely from scratch. We built it on top of the Monogame framework, which handles two main things: rendering and the game loop. Everything else, such as servers, textures, the game engine, physics, gameplay, and so much more was entirely up to us.

So, how did we do it?

<figure>
    <img class="responsive-image" src="../assets/img/projects/BFB/BFB_Player.png" alt="Player SpriteSheet" width="50%"   />
    <figcaption>Spritesheet for the player. Consists of an idle animation, and running animation.</figcaption>
</figure>
## Starting Off

At the beginning of the semester, we were sure that we wanted to make a game, however, we didn't really know what we wanted the game to be about. We created a game design document where we brainstormed everything we wanted to go into the game from how the players would feel to how gameplay would work. You can see that game design document below. You'll notice that this was just brainstorming everything we could come up with, and not everything was added.

<embed class="image-responsive" src="../assets/img/projects/BFB/BFB_GameOutline.pdf" type="application/pdf" width="800px" height="800px">

We decided that we wanted to initially start with a desktop application, and then toward the end of the semester, if we had time, we would port it to mobile. This meant that we needed to work in a framework that supported cross-platform compatibility. This is when we found Monogame. From there, we started to create the game.

## Workflow

Since this class also was meant to teach us about agile principles, we ended up working in five 3-week sprints. Rather than daily standups, we had one or two meetings per week. The work for each sprint was as follows:

### Sprint 1
- Experiments: Since we had no experience using Monogame, this sprint was designed for everyone to play around with the framework and see if it would fit our needs.

### Sprint 2
- *CI/CD* 
- Tile Map/Engine
- Player / Player controls
- Event Manager
- Scene Manager
- Basic TCP Server
- *Animation Manager*

### Sprint 3
- *Website / Database / User Authentication*
- UI Framework
- Human Physics
- Building
- Camera
- Chat

### Sprint 4
- Mockito Unit Tests
- UI Framework
- *Spells*
- *Combat*
- *Store*
- Monster Physics / AI
- Inventory

### Sprint 5
- Settings
- *Game Components / Gameplay*
- *Boss*
- *Update AI*
- HUD
- Audio
- Update Chat

As you can see, we completed a lot of work. Italicized bullets are ones that I specifically worked on.

## Final Product

When we were presenting to class, we created this poster to sum up our project.

<embed class="image-responsive" src="../assets/img/projects/BFB/BFB_FinalReport.pdf" type="application/pdf" width="800px" height="800px">

