# Editor overview

- [What is the Koji Editor](#editor)
- [Code hosting and source control](#source-control)
- [Environment and terminals](#environment)
- [Live preview](#live-preview)

## <a name="editor"></a>What is the Koji Editor?
The Koji Editor is browser-based integrated development environment (IDE). You can use the Koji editor to create apps and games. The editor contains all of your project's code, Visual Customization Controls (VCCs) generated from the project's code, publish/deploy management interfaces, and interfaces to other production tools like Koji Database, Custom Domains, Plugins, and more.

![alt text](/docs/02_editor/editor_overview/project_overview.png)

At a technical level, the Koji editor is a remotely-hosted Docker container running your project's code (e.g., frontend and backend development processes). That Docker container also contains an editor server injected by Koji, and your browser connects to that editor server (you can think of the Koji web editor as, in a sense, an "interactive live stream" of your development server).

When you open a file, press a key, or execute a terminal command, your browser is simply sending that action into the running development server. The development server is responsible for actually mutating files and managing state. You can see this in action by opening the same project in two separate windows; you will see the same files open in both tabs and two cursors in the code editor. This kind of collaboration is also available to remote users by clicking the "Collaborate" button on the top right of the editor.

Development servers are created on demand and automatically hibernate after 15 minutes of inactivity.

You can access the Koji editor from any device with a web browser, you do not need to install any additional third party software to use the web editor. This means you can remix, edit, and publish Koji apps on tablets and mobile phones, in addition to computers.

## <a name="source-control"></a>Code hosting and source control

Each Koji project is backed by a Git repository. When you remix a project, Koji clones that project's Git repository to your account and loads it in the editor. When you publish your app from the editor, Koji commits your changes and pushes to master. This push triggers a deploy. You can think of the state of your Koji editor in the same way as the state of a Git repository on your computer -- that is, it is a working HEAD of your repository. This means that if you make changes to your project from outside the Koji editor, you will need to `git pull` those changes from within the Koji editor in order to see them.

If you would like to use a remote editor, you will need to generate a Git Access Token from your Account Settings. You can use this access token in place of a password when cloning your repositories. To find the remote URL of your project's Git repository, navigate to "Settings" -> "Repository" from within the Koji editor.

## <a name="environment"></a>Environment and terminals

From the Koji web editor, you have access to an entire remote Linux development server. You can use the terminals at the bottom of the screen to run any commands you like.

## <a name="live-preview"></a>Live preview

### Inline preview
The Live Preview in the editor is a running preview of your app.

On the bottom of Live Preview, there are four buttons:
- “Phone” shows what your app will look like in portrait mode on a mobile device.
- “Tablet” shows what you app will look like on a tablet or large mobile device.
- “Embed” shows what your app will look like when it’s rendered on withkoji.com or embedded in a social network like Reddit.
- “Show Logs” opens a side panel showing the console logs of your project. This is useful for debugging your project at runtime when there are error or it is not behaving as intended.

### Remote preview
The "Remote" tab allows you to test your app in a new tab on it's own or on another device by scanning the QR code. You can even share the remote preview link with someone else, and they can see the preview as you develop. The remote preview link will only work for as long as your editor session is still active, so it is not suitable for long-term sharing.

Even when your app preview is open in another tab, you will still have access to the console logs of the remote device. This allows you test your app on different devices and computers while still working in the editor, including things like debugging different browsers, operating systems, and form factors.

![alt text](/docs/02_editor/editor_overview/remote_tab.png)

### <a name="route"></a>Route Tester
The "Routes" tab is used for testing backend routes. You can build your request and then execute it by pressing the “Send Request” button. The “Fetch” tab generates a code snippet for the current request that you can easily add to your app’s frontend.
