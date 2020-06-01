---
layout: project
permalink: /portfolio/FrameByFrame
---
<figure>
    <img class="responsive-image" src="../assets/img/projects/FrameByFrame/SquirrelHacks.gif" alt="Sample Animation"  width="50%" />
    <figcaption>A sample animation I created with Frame by Frame</figcaption>
</figure>

<svg class="svg-icon"><use xlink:href="{{ '/assets/minima-social-icons.svg#github' | relative_url }}"></use></svg> [View Source](https://github.com/danielbarnes175/FrameByFrame) 

# Frame By Frame

Frame By Frame is a simple application for creating frame by frame animations. This was created for the 2020 hackathon season (in November 2019, oddly enough) entirely in a single weekend. The program uses the Monogame framework since I needed something for rendering textures, and I had been working with Monogame during that time.

Monogame supplies two things: rendering and a game loop. The rendering was very helpful for creating the graphics of the program. The game loop was helpful for initial startup of functionality, but it ended up being limiting-- more on that later.

# A Drawing Program

This hackathon was the first one I participated in coming off of my first internship, where I had learned the concept of the MVP, or minimal viable product. So what is the minimal viable product for an animation program? That's right, it's a drawing program.

So what did I do? Well, there was a few boilerplate stuff that I needed. I needed a scene manager so that I could have a drawing scene and main menu. This was the first thing I started with.

I kept a dictionary of all my scenes like so:

```
 GlobalParameters.Scenes = new Dictionary<string, BaseScene>();

 GlobalParameters.Scenes.Add("Menu Scene", new MenuScene());
 GlobalParameters.Scenes.Add("Projects Scene", new ProjectsScene());
 GlobalParameters.Scenes.Add("Settings Scene", new SettingsScene());
 GlobalParameters.Scenes.Add("Drawing Scene", new DrawingScene());

 GlobalParameters.CurrentScene = GlobalParameters.Scenes["Menu Scene"];
```

I just needed to set `CurrentScene` whenever I wanted to change scene. I then rendered/updated based on the current scene:

`GlobalParameters.CurrentScene.Update(gameTime)` and `GlobalParameters.CurrentScene.Draw(Vector2.Zero)`

This was the scene manager. Pretty simple, but very effective. So in my drawing scene, I needed a way to draw. To do this, I kept lists of textures in "layers". When I needed a new layer, I would just create a new monogame texture at the position of the mouse click.

Call this function and add the returned value to the layer:

```
        public static Texture2D CreateTexture(GraphicsDevice device, int width, int height, Func<int, Color> paint)
        {
            //initialize a texture
            Texture2D texture = new Texture2D(device, width, height);

            //the array holds the color for each pixel in the texture
            Color[] data = new Color[width * height];
            for (int pixel = 0; pixel < data.Count(); pixel++)
            {
                //the function applies the color according to the specified pixel
                data[pixel] = paint(pixel);
            }

            //set the color
            texture.SetData(data);

            return texture;
        }
```

Then in my draw function, I iterate through each layer and the textures associated with them and redraw them.

```
            foreach (BasicTexture point in frames[currentFrame]._layer3)
            {
                point.Draw(new Vector2(5, 25));
            }

            foreach (BasicTexture point in frames[currentFrame]._layer2)
            {
                point.Draw(new Vector2(5, 25));
            }

            foreach (BasicTexture point in frames[currentFrame]._layer1)
            {
                point.Draw(new Vector2(5, 25));
            }
```

That's the basics of creating the drawing program. I won't go in depth about some additional features I added for that such as different colors, and an eraser tool.

# An Animation Program

Going from an animation program to a drawing program is actually quite simple. I need a way to keep track of layers for each frame. So I created a frame class:

```
public class Frame
    {
        public List<BasicTexture> _layer1;
        public List<BasicTexture> _layer2;
        public List<BasicTexture> _layer3;

        public Frame()
        {
            _layer1 = new List<BasicTexture>();
            _layer2 = new List<BasicTexture>();
            _layer3 = new List<BasicTexture>();
        }
    }
```

Then I just needed to keep individual frames in a list. A series of frames can be played in order to create an animation:

```
class Animation
    {
        public List<BasicTexture> Frames;

        public Animation()
        {
            Frames = new List<BasicTexture>();
        }
    }
```

We still need a way to animate them, however. We already know how to draw a single frame, or drawing, right? We do, so all we have to do is iterate through every frame in our animation and draw each of them.

```
public void Animate(GameTime gameTime)
        {
            if (timePlaying % fps != 0) return;
            currentFrame += 1;
            if (currentFrame > totalFrames-1)
                currentFrame = 0;
        }
```

What this code does is it looks at a timer, and if the timer hasn't reached our fps value, it will return, otherwise it will set the current frame to the next frame. This allows for us to iterate through all the frames by setting them as the current frame. Remember that the current frame is drawn in the draw method. The Animate function is called in the update method as follows:

```
if (isPlaying)
            {
                timePlaying += 1;
                Animate(gameTime);
                return;
            }
```

`isPlaying` is a boolean that, obviously, keeps track of if we are playing our animation.

With this, we have our animation program. Of course, there's some stuff needed for keeping track of key bindings for toggling `isPlaying` and moving through different frames, but that isn't important for this writeup.

# Exporting

We have our animation, but we don't yet have a way to export it.

```
 public void ExportAnimation()
        {
            for (int i = 0; i < frames.Count; i++)
            {
                RenderTarget2D texture = combineTextures(frames[i]);
                System.IO.Directory.CreateDirectory("Projects/" + projectName);
                SaveTextureAsPng("Projects/" + projectName + "/Frame_" + i + ".png", texture);
            }

            CreateGif("Projects/" + projectName + "/_" + projectName + ".gif");
            Console.WriteLine("Exported Animation as GIF to " + Directory.GetCurrentDirectory() + "Projects/" + projectName);
        }

        private void SaveTextureAsPng(string filename, RenderTarget2D texture)
        {
            FileStream setStream = File.Open(filename, FileMode.Create);
            StreamWriter writer = new StreamWriter(setStream);
            texture.SaveAsPng(setStream, texture.Width, texture.Height);
            setStream.Dispose();
            
        }

        private void CreateGif(string filename)
        {
            using (MagickImageCollection collection = new MagickImageCollection())
            {
                for (int i = 0; i < frames.Count; i++)
                {
                    collection.Add("Projects/" + projectName + "/Frame_" + i + ".png");
                    collection[0].AnimationDelay = 8;
                }

                collection.Write(filename);
            }
        }
```

I'll leave disecting this part to you. Basically what it does it converts each frame into a png and saves them to a folder for the specific animation. Then it uses the MagickImageCollection framework to create a gif from those pngs. It then saves that gif to the same project folder.

# Conclusion

I had been wanting to create an animation program for a long time before creating this. The hackathon finally gave me a good excuse to take a stab at it, and I succeeded. That's a win in my book. There are a ton of features I didn't talk too much about in this from the onion skin to the project viewer.

Of course, there is room for improvement. I was limited by the game loop being too slow which made drawing sometimes a pain. If you drew a straight line, there could be breaks in the line because you were moving your mouse faster than the game loop could update and capture your mouse position.

I also didn't account for a way to save animations and come back to work on them later. That's a huge oversight in my opinion, since animations aren't generally finished in one sitting. Maybe I'll come back and finish it one day. I also should probably clean up some of the code since I was working on it with minimal sleep.

tl;dr fun project.

<figure>
    <img class="responsive-image" src="../assets/img/projects/FrameByFrame/StickFigure.gif" alt="SampleAnimation"  width="50%" />
    <figcaption>Another sample animation I created with Frame by Frame</figcaption>
</figure>