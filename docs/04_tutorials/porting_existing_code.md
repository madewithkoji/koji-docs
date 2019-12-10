# Porting an Existing Game to Koji

## Overview

Goal: In this tutorial, we will port an existing game to Koji. We will make some updates to the game to make it customizable, and then we will build and deploy the game to the Koji platform.

Time: You should be able to complete this tutorial in 30 minutes or less.

Prerequisites: Familiarity with the Koji editor, remix process, `@withkoji/vcc` package, and basic git commands.

## Start with a Scaffold

Because every app on Koji is a remix, we will want to find a "good" place to start from. There are scaffolds for many existing game and app frameworks, including Phaser, P5, and React.

But because we will just be working with a vanilla JS game, we will want an even simpler starting point.

All games and apps on Koji will need to be "built" for production, meaning that they will need to be compiled into assets that can be bundled and served statically. One solution that is very good at doing that is Webpack.

Webpack will allow us to create a hot-reloading development environment, and will build our game for production as well. It also allows for transpiling of advanced JS features using babel. The worst part about webpack is configuring it properly; fortunately we have a scaffold that has already done that for you!

- Begin by creating a remix of this "Simple Webpack Scaffold": https://withkoji.com/~seane/simple-webpack-scaffold

- Verify that the project is working by opening up the "App settings" under the "Customization" section on the left hand side of the editor, and changing the "Hello, world!" value to something else.

- If everything is working correctly, you should see the preview on the right update automatically with your new value.

## Bringing in Existing Code

There are many ways to bring existing code into your project. If you have private files you would like to import, you could share them using a service and bring them in with something like `wget`.

For this project, however, we'll be importing files from a public git repository: https://github.com/arist0tl3/HTML5-Asteroids

This is a fork of a basic Asteroids clone made in vanilla JS: http://www.dougmcinnes.com/2010/05/12/html-5-asteroids

- Let's cancel the running npm command by clicking on the "frontend" terminal at the bottom of the editor screen, and pressing 'Ctrl-C'

- We'll clone our Asteroids repo into a temp folder: `git clone https://github.com/arist0tl3/HTML5-Asteroids temp`

- Let's move into the temp folder, using `cd temp` and move the important files into our parent folder: `mv game.js index.html ipad.js jquery-1.4.1.min.js vector_battle_regular.typeface.js ../`

- We can now move back to the parent folder, using `cd ..` and then remove the temporary folder using `rm -rf temp`

- The last step is to replace the existing index.js file with our new game.js file, so we can do `rm index.js && mv game.js index.js`

- Now if we re-run our start command, we should be able to refresh our preview and see our Asteroids game: `npm start`

<img src="" />

## Making it (More) Remixable

By design, every app and game on Koji is remixable. You can click the "Remix" button to create a forked version of the original.

But we want to take things one step further, by leveraging one of the most powerful tools on the platform: VCCs.

VCCs are Visual Customization Controls, and these will allow other people who remix your game or app to change things, without looking at one line of code. Another way to think about this is "theming" or "reskinning" an app -- the underlying code isn't changing, just the assets.

Let's say we want to make the background color of our game customizable. We'll need to do two things: set up a VCC to handle our user input, and consume the value inside our application.

- To set the VCC up, choose the "App settings" from the "Customization" menu on the left, and then at the top of the file, choose the "CODE" view

- Paste the code into the file, and then press Ctrl-S to save the file:

```
{
  "settings": {
    "backgroundColor": "#ffffff"
  },
  "@@editor": [
    {
      "key": "settings",
      "name": "App settings",
      "icon": "⚙️",
      "source": "settings.json",
      "fields": [
        {
          "key": "backgroundColor",
          "name": "Background Color",
          "type": "color"
        }
      ]
    }
  ]
}
```
You can see that we've replaced the existing value `name` because we won't need to use that value in our application.

- If you switch to the "VISUAL" view at the top of the file, you'll now see that the editor has rendered a color picker so that the user can choose a new background color.

If you change the background color, however, you will notice that nothing happens in our application. We need to set up our application to consume that value, which we can do using the `@withkoji/vcc` package.

This package has already been installed in this application, but if you need to use it in another project, you can just run `npm install @withkoji/vcc`

Because this project is written in vanilla JS, we will use some simple dom manipulation to make our background color changes.

- Open up the `frontend/index.html` file -- you will notice that there is a `canvas` element, with an id of `canvas` -- this is the element we will target for our background color

- Open up the `frontend/index.js` file, and add this to the top of the file (below the initial comment):

```
const Koji = require('@withkoji/vcc').default;

const canvas = document.getElementById('canvas');
if (canvas) {
    canvas.style.background = Koji.config.settings.backgroundColor;
}
```

What we are doing here is importing the default export from the `@withkoji/vcc` package, finding our canvas element, and assigning a value that is pulled from our configuration files.

If you refresh your application, you should now see the correct background color. If you return to the "App settings" customization, changing the background color should also update your preview automatically!

## Publishing

Because we are using Webpack to compile our project, we will need to take one extra step, and `require` the local JS files we are using, instead of referencing them in the index.html file.

- Open the `frontend/index.html` file, and remove the `<script>` tags from the top of the file. There should be four of them. Then press Ctrl-S to save the file.

- Open the `frontend/index.js` file, and add the following lines, just above where we pasted the last block of code:

```
require('jquery-1.4.1.min.js');
require('vector_battle_regular.typeface.js');
require('ipad.js');
```

- Press Ctrl-S to save the file, and make sure the preview is still working.

When you are ready to see a live build of your project, you can simply choose the "Publish now" link from the "Project" menu on the left hand side of the editor.

You will be prompted to enter some additional data about your application, and you can then click the "Publish app" button.

The build commands specified for this project will run, and you should end up with a live link to your production app.

## Extra Credit

At this point, you should have a live, published app on Koji! But there is something we can do to make it a bit nicer: adding some sound effects.

### Add Sounds

Let's start by getting our sounds. If you remember back to the beginning of this tutorial, we moved a few files from the original Asteroids repo into our project, but we didn't move the `.wav` files. That's because we want our users to be able to customize those sounds.

- Open up the "App settings" customization again, and navigate to the "CODE" tab, and replace the contents with the following:

```
{
  "settings": {
    "backgroundColor": "#d3c1c1",
    "laserSound": "",
    "explosionSound": ""
  },
  "@@editor": [
    {
      "key": "settings",
      "name": "App settings",
      "icon": "⚙️",
      "source": "settings.json",
      "fields": [
        {
          "key": "backgroundColor",
          "name": "Background Color",
          "type": "color"
        },
        {
          "key": "laserSound",
          "name": "Laser Sound",
          "type": "sound"
        },
        {
          "key": "explosionSound",
          "name": "Explosion Sound",
          "type": "sound"
        }
      ]
    }
  ]
}
```

- Press Ctrl-S to save the file, and then return to the "VISUAL" view. You should now see two more pickers, that will allow a user to choose sounds for your game.

- Open up the `frontend/index.js` file again, and press Ctrl-F to bring up the finder tool. Enter ".wav" (without the quotes) to search for the wav files that the game is using.

- Replace this block of code with the following:

```
SFX = {
  laser:     new Audio(Koji.config.settings.laserSound),
  explosion: new Audio(Koji.config.settings.explosionSound),
};
```

- Press Ctrl-S to save the file

Now our app will be referencing the values that are selected for those sounds. You can return to the "App settings" configuration, and use the sound pickers to replace these sounds with whatever you want!

You can even try just recording some sounds yourself -- I suggest "pew" for the laser and "pow!" for the explosion.

Changes to the sound choices should trigger a refresh of your application, and you should be able to hear the new sounds!

- Make sure to publish your new changes!

## Wrapping up

Hopefully this has given you a better understanding of how to bring existing code onto the platform, and integrate with the tools that make Koji awesome.

Reach out to @diddy on [Discord](https://discord.gg/eQuMJF6) if you have questions or comments about this tutorial =)
    
