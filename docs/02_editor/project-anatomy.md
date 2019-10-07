# Anatomy of a Koji Project

## Environment
Koji provides you with an environment for developing and publishing an app in your browser. This includes a terminal connected to a running container with all the familiar tools, the same code editor that powers VSCode, and tools to help you deploy and publish your app to [withkoji.com](https://withkoji.com/).
 
## Project Directory
The project directory at `/usr/src/app` includes a git repository for your project with two remotes.
1. **origin remote** is the git remote projects are built and deployed from.
2. **upstream remote** is the git remote projects are [forked from](https://help.github.com/en/articles/fork-a-repo) (a repository on github for example)

Run `git remote -v` to see the remotes in your project.
```sh
root@c8ce449b5538:/usr/src/app# git remote -v           
origin  https://projects.koji-cdn.com/8a129043-4c22-4be7-875f-38b422008695.git (fetch)
origin  https://projects.koji-cdn.com/8a129043-4c22-4be7-875f-38b422008695.git (push)
upstream        https://projects.koji-cdn.com/e4d6b4bf-781a-4a49-9f0f-1d9cf3bbb7ff.git (fetch)
upstream        https://projects.koji-cdn.com/e4d6b4bf-781a-4a49-9f0f-1d9cf3bbb7ff.git (push)
```
read instructions for syncing with an upstream repo [here](https://help.github.com/en/articles/syncing-a-fork)

## README
The `README.md` file is in your project directory and renders a markdown file for the **Overview** tab in the **Project** section on the left hand side.

## Koji Directory

The `.koji` directory is where your configuration and script files live. The including [project configuration](#kojiproject) for development and deploy and [customizations](#kojicustomization) for visual customization controls (vccs).

```sh
.
├── customization
│   ├── about_customization.md
│   ├── buttons.json
│   ├── colors.json
│   ├── images.json
│   ├── metadata.json
│   ├── settings.json
│   └── sounds.json
├── project
│   ├── about_project.md
│   ├── deploy.json
│   └── develop.json
```
⚠️ **The only required files for Koji to work are:**
- `README.md` displays project information in the project overview
- `develop.json` lets editor how your project development works (project path, port, start command, and events)
- `deploy.json` lets deploy know where your project build works (build path, port, build commands)

### **develop.json**
`.koji/project/develop.json` contains development configuration for your project.

```json
{
  "develop": {
    "frontend": {
      "path": "frontend",
      "port": 8080,
      "startCommand": "npm start"
    }
  }
}
```
In this file the important things to look at are:
1. `"path"` leads to the correct directory for your frontend and backend
2. `"port"` in this file matches up with the one your development server is running on.
3. The `"startCommand"` for your app is placed in the start script of your `package.json` (for npm start to work)

### **deploy.json**
`.koji/project/deploy.json` contains deploy configuration for your project.
```json
{
  "deploy":  {
    "frontend":  {
      "output":  "frontend/build",
      "commands":  [
        "cd frontend",
        "npm install",
        "export NODE_ENV=production && npm run compile"
      ],
    },
    "backend":  {
      "output":  "backend/dist",
      "type": "dynamic",
      "commands":  [
        "cd backend",
        "npm install",
        "export NODE_ENV=production && npm run compile"
      ]
    }
  }
}
```
In `deploy.json` make sure to check that:
1. Your `"output"` goes to the directory where your files will be compiled into after the `"commands"` list is done.
2. `"type"` is either `"static"` if the service deploys a static bundle (HTML, CSS, JS), or `"dynamic"` if the service deploys a server.
2. `"commands"` is the complete list of steps in order to build your project.

The `.json` files in this directory each have a very specific schema that controls how the Koji editor deals with files. `develop.json` deals with setting up your editor so you will have the best editing experience and `deploy.json` tells Koji how to turn your code into a live app. These files have the following schema:

### .koji/customization
Every `.json` file in this directory is created by the template creator and creates a tab in the **Customization** section in the navigation bar on the left side of the screen. 
The following would create a section called **Example** and would be in a file called `example.json`:
```json
{
  "example":  {
    "param":  "this is the value of the param"
  },
  "@@editor":  [
    {
      "key":  "example",
      "name":  "Example",
      "icon":  "😄",
      "source":  "example.json",
      "fields":  [
        {
          "key":  "param",
          "name":  "An Example Parameter",
          "type":  "text"
        }
      ]
    }
  ]
}
```
If you are using the [@withkoji/vcc](https://github.com/madewithkoji/koji-vcc) npm package, you can access this Customization property with `Koji.config.example.param`.
