# Anatomy of a Koji Project <!-- omit in toc -->

## Table of Contents <!-- omit in toc -->
- [Environment](#environment)
- [Project Directory](#project-directory)
- [README](#readme)
- [Koji Directory](#koji-directory)
  - [**develop.json**](#developjson)
  - [**deploy.json**](#deployjson)
  - [.koji/customization](#kojicustomization)
  - [.koji/scripts](#kojiscripts)
  - [.koji/hooks](#kojihooks)
- [The *frontend* directory](#the-frontend-directory)
  - [frontend/package.json](#frontendpackagejson)
  - [frontend/.internals](#frontendinternals)
  - [frontend/common](#frontendcommon)
  - [frontend/pages](#frontendpages)
    - [frontend/pages/AnyDirectoryName](#frontendpagesanydirectoryname)
  - [*variations in frontend structure](#variations-in-frontend-structure)
- [The *backend* directory](#the-backend-directory)
  - [backend/package.json](#backendpackagejson)
  - [backend/index.js](#backendindexjs)
  - [backend/routes](#backendroutes)
  - [backend/routes/RouteNameHere](#backendroutesroutenamehere)
- [Questions / Ideas / Fixes](#questions--ideas--fixes)

## Environment
Koji provides you with an environment for developing and publishing an app in your browser. This includes a terminal connected to a running container with all the familiar tools, the same code editor that powers VSCode, and tools to help you deploy and publish your app to [withkoji.com](https://withkoji.com/).
 
## Project Directory
The project directory at `/usr/src/app` includes a git repository for your project with two remotes.
1. **origin remote** is the git remote projects are built and deployed from.
2. **upstream remote** is the git remote projects are forked from (a repository on github for example)

Run `git remote -v` to see the remotes in your project.
```sh
root@c8ce449b5538:/usr/src/app# git remote -v           
origin  https://projects.koji-cdn.com/8a129043-4c22-4be7-875f-38b422008695.git (fetch)
origin  https://projects.koji-cdn.com/8a129043-4c22-4be7-875f-38b422008695.git (push)
upstream        https://projects.koji-cdn.com/e4d6b4bf-781a-4a49-9f0f-1d9cf3bbb7ff.git (fetch)
upstream        https://projects.koji-cdn.com/e4d6b4bf-781a-4a49-9f0f-1d9cf3bbb7ff.git (push)
```

## README
The `README.md` file is in your project directory and renders a markdown file for the **Overview** tab in the **Project** section on the left hand side.

## Koji Directory

The `.koji` directory is where your configuration and script files live. The including [project configuration](#kojiproject) for development and deploy, [scripts](#kojihooks), and [customizations](#kojicustomization) for visual customization controls (vccs).

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
├── hooks
│   ├── about_hooks.md
│   └── post-clone.sh
├── project
│   ├── about_project.md
│   ├── deploy.json
│   └── develop.json
└── scripts
    ├── buildConfig.js
    └── buildManifest.js
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
      "events": {
        "started": "[webpack] Frontend server started",
        "building": "[webpack] Frontend building",
        "built": "Compiled successfully.",
        "build-error": "[webpack] Frontend build error"
      },
      "startCommand": "npm start"
    }
  }
}
```
In this file the important things to look at are:
1. `"path"` leads to the correct directory for your frontend and backend
2. `"Port"` in this file matches up with the one your development server is running on.
3. the `"startCommand"` for your app is placed in the start script of your `package.json` (for npm start to work)
4. Your development server prints out the string in `"built"` when your files are ready to be displayed.

### **deploy.json**
`.koji/project/deploy.json` contains deploy configuration for your project.
```json
{
  "deploy":  {
    "subdomain":  ".withkoji.com",
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
1. your `"output"` goes to the directory where your files will be compiled into after the `"commands"` list is done.
2. `"commands"` is the complete list of steps in order to build your project into pages that can be served statically.


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
If you are using the [koji-tools](https://www.npmjs.com/package/koji-tools) npm package, you can access this Customization property with `Koji.config.example.param`.

### .koji/scripts
This directory holds internal tools for dealing with Koji related tasks. Right now the only important file that lives in this directory is `buildManifest.js`, which is used to turn your project metadata into a `manifest.json` file which is needed in order for apps to be PWA's.

### .koji/hooks
The hooks directory is where bash scripts that are automatically run after different Koji Editor events take place. These are currently not well documented and I don't recommend their use right now.

## The *frontend* directory
The frontend of your project will live in this directory. Specific files in this directory are specific to the javascript library you are writing in. Throughout any javascript framework, structurally, the following files and folders should exist at the following places. If they are in different places, there should be a good reason given the setup of the javascript framework you are using.

### frontend/package.json
Your standard `package.json` for a node.js project. 

### frontend/.internals
Any files that are required for the frontend to run but are not worth touching in regular development should be in this directory.
Things like:
- Webpack configuration files
- Service worker initialization files
- Framework specific config files

### frontend/common
Any files that do not render elements directly to the page of your app but exist across every page of your project should be in common. 
Things like:
- Global CSS files
- A file that handles page routing
- A main App.js file
- Your index.html

### frontend/pages
This is where the main code for your project goes. Each page of your app should be in a different directory inside of the pages directory. 

#### frontend/pages/AnyDirectoryName
These are the pages of your app. These pages should normally have at least an `index.js` and a `koji.json`. The `index.js` should have the code that includes all of the files you need for the page and sets up the page. The `koji.json` should have this format:
```json
{
  "pages":  [{
    "name":  "Page Name Here",
    "route":  "/",
    "path":  "frontend/pages/AnyDirectoryName"
  }]
}
```

### *variations in frontend structure
Some possible changes that could be made to the `frontend` file structure given different javascript frameworks.
- If you are only going to use one page you might consider something like a `/app` directory instead of a `/pages` directory. 
- If your configuration files need to be changed around alot it might make more sense to get rid of the dot in `/.internals` to be just `/internals` to show that those files should be edited.

## The *backend* directory
The backend routes for your project are in this directory. This directory is more or less the same for most of the projects that use a backend on Koji. Most projects use a standard express server to serve a set of api endpoints. 
**\*These routes get built into serverless endpoint (AWS Lambda), so currently long polling requests and websockets are not supported.**

### backend/package.json
Just like with frontend, a regular node.js `package.json` file that manages your node project.

### backend/index.js
This file loads all of your routes in the `backend/routes` directory explained below and handles putting these routes on a serverless endpoint on deploy.

### backend/routes
Just like frontend has pages, backend has routes, each route should have its own folder.

### backend/routes/RouteNameHere
Each route should have at least two files: 
- `index.js`: which exports what code the route should be executing.
- `koji.json`: which should have the following format -
```json
{
  "routes":  [{
    "name":  "RouteNameHere",
    "path":  "backend/routes/RouteNameHere",
    "route":  "/example/route",
    "method":  "GET"
  }]
}
```

## Questions / Ideas / Fixes
If you have any questions or ideas on how to make Koji better please:
- [join our discord server](https://discord.gg/eQuMJF6)
