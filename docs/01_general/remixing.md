# Remixing Apps

## What is remixing?

Every app in Koji is a remix of another app. You can remix anything, from a barebones scaffold to a fully-featured 3D game, and everything in between. Remixing an app gives you full access to all the code in the Koji editor, Koji Visual Customization Controls (VCCs) that make it easy to customize an app without ever touching any code, and a fully-automated, one-click deploy tool to publish your remix as a new, standalone app.

The first step when remixing an app is ask yourself, “What type of app do I want to make?”. The easiest way to get inspired is to browse the [“Create” page](https://withkoji.com/create) or the [“Start Remixing” page](https://withkoji.com/tag/start-remixing). Simply find a game you like, click on the “Remix” button, and start modifying it! There are many types of apps and games already on Koji, and it’s likely there is already another app that is similar to what you wish to make. You can remix it and build from there..

The rest of this help document is intended for developers, or people who are already familiar with coding, and want to learn a bit more about how remixing works and how to build projects that are remixable on Koji. If you’re looking for a quick tutorial that will get you up and running remixing games, check out this [Youtube tutorial](https://www.youtube.com/watch?v=xMPvUkkcpac).

## Creating a “blank” project

If you want a blank project, the easiest way is to remix a simple app or scaffold,, as it is already packaged with the required files and setup to work on Koji. One of the lightest-weight scaffolds, with only the basic files needed to work with Koji, is the “Blank Starter Parcel” created by @gron: https://withkoji.com/~ron/blank-starter-parcel.

There are two approaches to easily create a new app. You can remix a Scaffold and build up from that, or you can take an already completed app and strip it down by removing what you don't need and adding what you do.

A list of community-created scaffolds can be found here: https://withkoji.com/tag/scaffolds-(for-developers)

The overview below uses a [P5 Scaffold](https://withkoji.com/~Svarog1389/p5-scaffold-with-leaderboard), but you are not limited to P5. Koji supports any Javascript frameworks, like React, Phaser, and even vanilla HTML5. If the framework you wish to use is not already available as a Koji scaffold , feel free to remix a different scaffold and  and modify it to include the framework you wish to use. 

The scaffold we’ll be using in this tutorial is Svarog’s “P5 Scaffold with Leaderboard,” which can be found here: https://withkoji.com/~Svarog1389/p5-scaffold-with-leaderboard

![alt text](/docs/01_general/remixing/p5_scaffoldWithLeaderboard.png)

On the bottom right of an app, you see a button labelled “Remix.” Once clicked, that button will create a new project for you to work in. 

After you click remix, you’ll see a loading screen as the project is built for you and the environment is configured. This can take above a minute. If you’d like to learn more about what’s happening behind the scenes when you remix a project, and how we use Git to facilitate remixing, check out our [Tech Deep Dive Video](https://www.youtube.com/watch?v=BgpFGMH6VR4).

Once the editor has loaded, you will see the “Project Overview” page. This will give you an overview of what the project does and often how it works as well. The project overview page is provided by the app developer through the inclusion of a “README.md” file.

![alt text](/docs/01_general/remixing/project_overview.png)

On the left of the screen you will see a directory of the entire project. The directory is broken into four main sections,
- “Project” which contains the overview and the section to publish your app to the web.
- “Customization” which contain customization controls that allows you or others remixing your project to edit variables of your project without changing any code, such as colors, images, sounds, strings, booleans, etc.
- “Code” which contains all of the code that makes up the project. Frontend is usually the main section where apps are developed in.
- “Advanced” which contains tools and settings for the project.

![alt text](/docs/01_general/remixing/project_directory.png)

On the right, you will see the “Live Preview” which shows the running application. You can interact with it while you develop, and see your changes in real time.
- “Live Preview” shows your current app running. On the bottom of Live Preview, there are four buttons,
  - “Phone” shows what your app will look like in portrait mode on a mobile device.
  - “Tablet” shows what you app will look like on a tablet or large mobile device.
  - “Embed” shows what your app will look like when it’s rendered on withkoji.com or embedded in a social network like Reddit.
  - “Show Logs” opens a side panel showing the console logs of your project. This is useful for debugging your project at runtime when there are error or it is not behaving as intended.
 
![alt text](/docs/01_general/remixing/live_preview.png)
 
The top section of the preview pane allows you to select the preview type, Next to “Live Preview” there is “Remote”, which allows you to test your app in a new tab on it's own or on another device by scanning the QR code. You can even share the remote preview link with someone else, and they can see the preview as you develop.

Even when your app is running remotely, you will still have access to the console logs of the remote device.This allows you test your app on different devices and computers while still working in the editor, including things like debugging different browsers, operating systems, and form factors.

![alt text](/docs/01_general/remixing/remote.png)

Next to “Remote” is the “Routes” tab. This is used for testing backend routes. You Can build your request and then execute it by pressing the “Send Request” button. The “Fetch” tab generates a code snippet for the current request that you can easily add to your app’s frontend.

![alt text](/docs/01_general/remixing/routes.png)

Remember to frequently test your app with different resolutions, different devices and different browsers. Your app should be responsive and work on both desktop and mobile devices.

## Editing a project

Before starting the coding section, remember a few requirements for making a great app:
- Apps should include VCCs that allow others to remix and customize your app in useful ways, such as sounds, images, strings, etc.
- If the app is a game or has a scoring system, it should include a backend with a leaderboard. Leaderboard help page can be found here: https://withkoji.com/docs/misc/leaderboard-guide 
- If the app has audio or music, it should not start playing until the user starts the app.
- Use high-quality assets as it will bring more people to your app. Remember, visuals are the first that people will notice about your app.

## VCC (Virtual Customization Controls) Data

To learn how to set up VCCs, and to learn more about the types of variables that can be used, read the VCC Reference help page here: https://withkoji.com/docs/editor/vcc-reference 

To access the the VCC values from code you will need to know the ‘key’ for the specific VCC you are trying to access. This can be found on the VCC page In the example below, under “Title”, you see a “strings.title”. The key will be the first text before the ‘.’. In this case, the key is ‘strings’.

![alt text](/docs/01_general/remixing/strings_title.png)

You can also find the key by viewing the code for the VCC. Within the code, you will see the key, as shown below. The text before all the variables is your key, it can also be found below under “@@editor/key”.

![alt text](/docs/01_general/remixing/game_settings.png)

To get the value from those variables within your code you will need to call, Koji.config.”yourKeyHere”.”variableName”. That will return the variable value that is set in the VCC.

For example, if you wanted  to access the “Title” variable from the VCC above,you would call “Koji.config.strings.title” in your code and it will return the string value of “P5 Scaffold with Leaderboard”.

## Coding

There are several ways you can work with your new project, such as cloning the git repository and working with your own integrated development environment (IDE). For the purposes of this document, we will be using the Koji Web Editor.

### Adding new files
You can add new files in one of several ways:
- Click the large blue '+' next to “Code” which will open a popup asking where you will type the path for the file.
- Highlight a folder in the directory and click on the blue plus that appears next to it which will open a popup asking for the filename.
  - The filename must also include the extension, i.e. '.js'.
  - If want to add a subfolder, just include that with the filename, i.e. “newfolder/newFile.js”.
- You can use the terminal to add a file using “touch newfile.js” Remember that this will need to include the full path for the file, such as “touch frontend/app/newfile.txt”
- You can use the terminal to add a folder using “mkdir new_Dir”, remember that this will need to include the full path for the folder, such as “mkdir frontend/app/new_Dir”

This project runs its core setup through the frontend/app/index.js file, so this is the place where we will start. You can build you app within this index.js file or add new files and then call them through the index file.

![alt text](/docs/01_general/remixing/index_file.png)

This being a P5 app, it has preload, setup and draw functions that handle the loading, main startup and drawing/updating the app. Most projects have a similar setup to this where the main bulk of the updating and drawing is done in the index.js file.

Remember to keep your code clean, organized and well commented as others will be remixing your app and they need to be able to follow along with your code so they can make their own changes or extend on your app.

Another thing to keep in mind is that any variables that can easily change the look and feel of your app should be accessible through the VCCs. You want your app to be easily remixed by those who have no coding experience and wish to use your app to create a version of their own. Examples would be: images, instructions, colors, speed, health, lives, etc.

## Project Overview

Once you have your app setup and function how you wish, move on to the “Project Overview” page of your app, which is located on the top of your directory under “Project”. 

![alt text](/docs/01_general/remixing/project_overview.png)

The project overview page is what the name describes, it is an overview of your project. It should describe what your app does and brief description on how it functions. This is where you can add notes to let others who are remixing your app what they can do, what’s already included or any other helpful information that you think could be needed.

Swap the view to “Code” view.

![alt text](/docs/01_general/remixing/project_overview_visual.png)

![alt text](/docs/01_general/remixing/project_overview_code.png)

The project overview is a Markdown file, so if you’re unfamiliar with it’s setup you can read about here: https://guides.github.com/features/mastering-markdown/ 

## Publishing

You have finished your app and it’s ready for everyone to use and remix themselves. So now the most important part, publishing your app to Koji. This is a simple and straightforward process. First, in your directory under “Project Overview” you’ll see “Publish.” Click on it and you’ll be brought to the publish page.

![alt text](/docs/01_general/remixing/publish_now.png)

![alt text](/docs/01_general/remixing/publish_app.png)

Here you will need to add a thumbnail, set your app name along with a description. Below are discovery and remix settings which are optional to use, but can help others find your app using only key words. Those will be gone over shortly, but first the “General Information”.

The name of your app should be creative and attention grabbing, along with your thumbnail it’ll be one of the first things someone sees about your app so you want to grab their attention immediately.

The description of your app should be meaningful, but not too detail specific. Remember, not everyone is a developer so make it general and describe your app how you would see a description in an app store. 

thumbnail of your app can be a static image or it could be a gif to show usage or gameplay of your app. How to create an animated thumbnail can be found here: https://blog.withkoji.com/make-your-koji-game-thumbnail-animated/ 

If you don’t choose to add a thumbnail, Koji will take a screenshot of your app after it’s been deployed and use that.

If you click on the “Discovery Settings” tab, it’ll open this panel

![alt text](/docs/01_general/remixing/discovery_settings.png)

Here you can add tags that’ll help others find you app easier, even if they don’t know the name.

Player Namespace is experimental right now so it can be ignored.

You can also make your app unlisted, in case you only want those who have the URL to see and use it.

If you click on the “Remix Settings” tab, it’ll open this panel

![alt text](/docs/01_general/remixing/remix_settings.png)

Here you can specify whether you want other people to be able to remix your app.

Once finished with the title, thumbnail, description and settings you can press the “Publish app” button. This will create and deploy your app to the Koji where if set to be listed will appear on the new page of Koji, https://withkoji.com/new. 

If you have chosen to have your app to be unlisted, it will still be deployed to Koji, but will not be shown to others. You can access your app under the directory you will now see “Open published app” available to click. This will take you to your deployed app where you can get and share the app URL with others.

![alt text](/docs/01_general/remixing/overview_directory.png)

## Additional reference
- https://withkoji.com/docs
- https://withkoji.com/docs/editor/vcc-reference
