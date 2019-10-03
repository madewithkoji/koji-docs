# Editor overview

- What is the remote editor

- Provisioning a dev server

- Terminals, live preview, "your computer in the cloud"

- Staging & live preview, remote debugging

- Route tester

- VCCs vs. code


## What is the remote editor?
The remote editor or Koji Editor is the online editor that appears when you open your game from the project menu. This is where you create your game, it contains code needed for the VCC’s, the game, plugins and leaderboard if necessary. The remote editor works on mobile devices as well so you can develop anywhere.


## Provisioning a Dev Server
A dev server is created when you remix any game. Your game will be backed up on GitHub and the link to the repository on GitHub can be found under Advanced/Setting/Repository.

## Terminals, Live Preview, “Your computer in the cloud”
Terminals are located on the bottom of the project page.

![alt text](/docs/02_editor/editor_overview/project_overview.png)

Opening a project will have a frontend and backend terminal open by default. More can be opened by clicking the “New Tab” button. 
- If you close the frontend or backend terminal and want to bring back, you go to “Settings/Development” and press “Force Restart Project”. This will take about 30 seconds and will have the frontend and backend terminals open again.
The Live Preview is a running preview of your app. You can swap from a phone view to either a tablet or embedded view. This is for you to test your app on different resolutions as your app should be built to support all sizes and resolutions.

## Staging & Live Preview, Remote Debugging
This is where you can test and debug your game as you are working on it.

Live Preview” shows your current app running. On the bottom of Live Preview, there are four buttons,
- “Phone” shows what your app will look like in portrait mode on a mobile device.
- “Tablet” shows what you app will look like on a tablet or large mobile device.
- “Embed” shows what your app will look like when it’s rendered on withkoji.com or embedded in a social network like Reddit.
- “Show Logs” opens a side panel showing the console logs of your project. This is useful for debugging your project at runtime when there are error or it is not behaving as intended.

Remote allows you to test your app in a new tab on it's own or on another device by scanning the QR code. You can even share the remote preview link with someone else, and they can see the preview as you develop.

Even when your app is running remotely, you will still have access to the console logs of the remote device.This allows you test your app on different devices and computers while still working in the editor, including things like debugging different browsers, operating systems, and form factors.

![alt text](/docs/02_editor/editor_overview/remote_tab.png)

## Route Tester
Routes is used for testing backend routes. You Can build your request and then execute it by pressing the “Send Request” button. The “Fetch” tab generates a code snippet for the current request that you can easily add to your app’s frontend.

## VCCs Vs. Code
VCC's are Visual Customization Controls that allow another person who will remix your project to easily modify a game or app to look and feel how they want it to. Games should have as much of their variables open to VCC so others can customize the game as much as they want. The games should be easy to customize without the need to change any code if possible. 
The code of your games should also be clean, easy to read and commented well. When other developers remix your game you want them to be able to easily follow along with what your setup is and the flow of the game setup.
