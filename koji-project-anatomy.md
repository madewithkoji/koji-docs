# Anatomy of a Koji Project
This is a list of where every file that is important to Koji lives and a description of what those files do. This is the recommended structure for Koji projects, deviations from this file organization are fine, but they should have a good reason.

**The only truly required files for Koji to work are in the first two sections, `README.md`, `develop.json`, and `deploy.json`.**
 After those files this document just describes best practices and what to expect in the Scaffolds and Templates that currently exist in Koji.
 
## ./README.md
This file is in your root directory and renders a markdown file for the *Overview* tab in the *Project* section on the left hand side.

## The *.koji* directory

This is where all of your configuration files live. There are a number of sub-directories here, each one controls a different part of Koji. 
### .koji/project
The `.json` files in this directory each have a very specific schema that controls how the Koji editor deals with files. `develop.json` deals with setting up your editor so you will have the best editing experience and `deploy.json` tells Koji how to turn your code into a live app. These files have the following schema:
#### .koji/project/develop.json
```json
{
  "develop":  {
    "frontend":  {
      "path":  "frontend",
      "port":  8080,
      "events":  {
        "built":  "Compiled successfully.",
      },
      "startCommand":  "npm start"
    },
    "backend":  {
      "path":  "backend",
      "port":  3333,
      "startCommand":  "npm start",
      "events":  {
        "started":  "[koji] backend started",
        "log":  "[koji-log]"
      }
    }
  }
}
```
In this file the important things to look at are:
1. `"path"` leads to the correct directory for your frontend and backend
2. `"Port"` in this file matches up with the one your development server is running on.
3. the `"startCommand"` for your app is placed in the start script of your `package.json` (for npm start to work)
4. Your development server prints out the string in `"built"` when your files are ready to be displayed.

#### .koji/project/deploy.json
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

### .koji/customization
Every `.json` file in this directory is created by the template creator and creates a tab in the *Customization* section in the navigation bar on the left side of the screen. 
The following would create a section called *Example* and would be in a file called `example.json`:
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
If you have any questions or ideas on how to make Koji better please either:
- [send me a message](https://gokoji.com/profile/jones)
or 
- [join our discord server](https://discord.gg/eQuMJF6)
