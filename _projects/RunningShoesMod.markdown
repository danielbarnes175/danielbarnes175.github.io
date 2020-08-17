---
layout: project
permalink: /portfolio/RunningShoesMod
---
# Minecraft Running Shoes Mod

Minecraft Running Shoes is a simple mod for Minecraft that adds shoes that make you run faster. I felt slow in my gameplay, so I wanted an easy way to go faster, while also still being balanced.

Check out a video demo [here](https://www.youtube.com/watch?v=6xJ0J0hItdI&t=0s)  
Check out the Curseforge page [here](https://www.curseforge.com/minecraft/mc-mods/running-shoes-mod-1-12-2)  

<svg class="svg-icon"><use xlink:href="{{ '/assets/minima-social-icons.svg#github' | relative_url }}"></use></svg> [View Source](https://github.com/danielbarnes175/MinecraftRunningShoesMod) 

As of writing this (August 2020), my running shoes mod has over 7,000 downloads. That's crazy! I never thought that this mod would be downloaded that much.

### Conception

I wanted to create a Minecraft mod, but unfortunately, tutorials on how to do this were few and far between. I managed to find one targeting Minecraft v1.12.2, which was the version I wanted to create my mod for. I watched the tutorial, but it only really showed how to add custom items.

I wanted a custom effect for my item. I wanted it to give the user a higher base speed. Initially I decided to have it apply a potion effect to the user, but then I realized that I want speed potions to be able to stack with my shoes. If the shoes themselves applied an effect, then this wouldn't be feasible. So, I switched to modifying the player movement speed. This was actually harder than it sounds because there wasn't any documentation. I had to look through the Minecraft forge api, find where they modify the player, and hope that there was something related to speed. Luckily for me, there was.

### Updates

I had hit my minimal viable product in simple running shoes, but I also wanted to add a config in case a user wanted to modify the speed of the shoes. This allowed users to tweak the mod to their liking. I think it's very important that a product allows a user to have some sense of control, so that's why I added them.

I also added sprinting shoes, which were the exact same as running shoes, but faster. They were also customizable in the config.

I also wanted to update the mod for more versions of Minecraft as they came out. Unfortunately, this ended up being a bigger undertaking than I had expected, and I simply didn't have the time and resources to dedicate to it, so currently it only supports 1.12.2.
