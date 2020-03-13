# Tutorial: Adding a Service to Your Project

## AKA: How the Heck Does "serviceMap" Work?

As you work through and extend templates on Koji, you will likely run into issues where your `Koji.config.serviceMap` doesn't look the way you think it should.

Or, you have remixed a project with a `frontend`, but you need to add a `backend`.

## Overview

**Goal**: The goal of this tutorial is to help you add a `backend` service to an existing project, and confirm that it is available in the `serviceMap`.

**Time**: You should be able to complete this tutorial in 30 minutes or less.

**Prerequisites**: Familiarity with the Koji editor, remix process, and `@withkoji/vcc` package is required. Familiarity with React and ES6 basics are a plus. 

## Tutorial

### Setting up the app

In order to add and test a `backend` service, we'll remix a project that already has a frontend:

https://withkoji.com/~seane/simple-react-scaffold

- Remix this project

- Once your editor has loaded, rename this project, to something like "Adding a Backend Project"

#### Watching for "serviceMap" changes

We will want to keep an eye on the value of `Koji.config.serviceMap`. Once we are able to see the `backend` service, we'll know that we've set up service properly.

- Open up `frontend/common/App.js` (our main client file)
- Inside the `render` method of the `App` component, add the following line just above the `return` call: `console.log('serviceMap:', Koji.config.serviceMap);`
- In the Preview window on the right of the screen, choose the "Remote" tab, and then click on "Open In New Tab"
- In your new tab, open your developer tools and confirm that you can see a console log of  your current `serviceMap`
- It should look like this:
```
serviceMap: {frontend: "https://8080-334797ce-40e0-4c76-af94-79fba39fff89.koji-staging.com"}
```

You can see that right now, we only have a `frontend` service. Next, we'll add a `backend`.

#### Adding the backend project

Our backend will be an npm project, just like our frontend. We'll be using just three files to get started, a `package.json` file that will define all of our packages and commands, a `.babelrc` file that will help with the transpiling, and a `server.js` file that will run a simple express server that our `frontend` will be able to call.

- On the left hand side of the editor, next to the "CODE", you'll see a plus that will allow you to add a file. Click that and then enter the following path:

```
/backend/package.json
```

That will create a backend folder, and a `package.json` file. 

- Open that file up, and then paste (and save) the following contents:

```
{
  "name": "koji-project-backend",
  "version": "1.0.0",
  "scripts": {
    "compile": "babel . -d dist --copy-files --ignore \"node_modules/**/*.js\"",
    "start-dev": "NODE_ENV=development babel-watch -L --watch ../.koji/ ./server.js",
    "start": "NODE_ENV=production node dist/server.js"
  },
  "dependencies": {
    "babel-polyfill": "^6.26.0",
    "body-parser": "^1.18.3",
    "cors": "^2.8.5",
    "express": "4.16.3",
    "uuid": "^3.3.2"
  },
  "devDependencies": {
    "@babel/cli": "7.2.3",
    "@babel/core": "7.2.2",
    "@babel/preset-env": "7.3.1",
    "babel-plugin-dynamic-import-node": "1.2.0",
    "babel-plugin-transform-object-rest-spread": "6.26.0",
    "babel-preset-env": "1.7.0",
    "babel-preset-stage-0": "6.24.1",
    "babel-watch": "git+https://github.com/kmagiera/babel-watch.git"
  }
}
```

Let's install our required packages.

- Open a new terminal by clicking on "New Tab" at the bottom of the editor screen

- You should see a new command line interface, open to `/usr/src/app`. Move into the backend folder by running `cd backend`

- Install the packages by running `npm install`

You should see many things running on the screen, and after a few minutes, you should be returned to the command line.

- The packages are now installed, and you can run `exit` to close the terminal.

- You can minimize the terminal window using the down caret `V` on the right hand side of the terminal window

Let's also add some babel configurations to help our project build and compile properly.

- On the left hand side of the editor, next to the "CODE", you'll see a plus that will allow you to add a file. Click that and then enter the following path:

```
/backend/.babelrc
```

That will create a `.babelrc` file in the `backend` folder. 

- Open that file up, and then paste (and save) the following contents:

```
{
  "presets": ["@babel/preset-env"],
  "plugins": ["@babel/plugin-proposal-object-rest-spread"]
}
```

That will help us use some handy syntax in our files that babel will compile for us!

So we've created our `backend` project, now let's add our `server.js` file so that we can run our server.

- On the left hand side of the editor, next to the "CODE", you'll see a plus that will allow you to add a file. Click that and then enter the following path:

```
/backend/server.js
```

That will create a `server.js` file in the `backend` folder. 

- Open that file up, and then paste (and save) the following contents:

```
import 'babel-polyfill';
import express from 'express';
import * as fs from 'fs';
import bodyParser from 'body-parser';
import cors from 'cors';

// Create server
const app = express();

// Specifically enable CORS for pre-flight options requests
app.options('*', cors())

// Enable body parsers for reading POST data. We set up this app to 
// accept JSON bodies and x-www-form-urlencoded bodies. If you wanted to
// process other request types, like form-data or graphql, you would need
// to include the appropriate parser middlewares here.
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({
  limit: '2mb',
  extended: true,
}));

// CORS allows these API routes to be requested directly by browsers
app.use(cors());

// Disable caching
app.use((req, res, next) => {
  res.header('Cache-Control', 'private, no-cache, no-store, must-revalidate');
  res.header('Expires', '-1');
  res.header('Pragma', 'no-cache');
  next();
});

app.get('/', async (req, res) => {
    res.status(200).json({
        text: 'Hello, world!',
    });
});

// Start server
app.listen(process.env.PORT || 3333, null, async err => {
    if (err) {
        console.log(err.message);
    }
    console.log('[koji] backend started');
});
```

This code will start a basic express server, and will respond to a `GET` with some basic JSON that we will be able to verify on the `frontend`.

#### Letting Koji know we've added a service

So we have added a `backend` project, but the Koji editor and project builder still don't know it exists.

The way that we can let Koji know is by updating the `develop.json` and `deploy.json` files. 

Under the CODE section, look for the `.koji` folder and open it. Inside of the `project` folder, you should see the two files we are looking for.

Let's update the `develop.json` file first, which helps the Koji editor know which services to run.

- Open the `develop.json` file and update the file to look like the code below:

```
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
    },
    "backend": {
      "path": "backend",
      "port": 3333,
      "startCommand": "npm run start-dev",
      "events": {
        "started": "[koji] backend started",
        "log": "[koji-log]"
      }
    }
  }
}
```

You can see that we have added an additional entry, the `backend` entry. Let's see if it worked!

- On the left of the editor, navigate to Advanced > Remote environment and choose "Force Restart Project (30 seconds)" under the Actions section

If everything went well, you should be able to reconnect to the editor and now see a "Backend" terminal has been added to the terminals at the bottom of the screen!

If you see `[koji] backend started`, your backend service is now running in your local editor environment!

- Let's confirm that the `serviceMap` is now populated by opening up our `frontend` preview tab and refreshing. You should now see a console log that looks like this:

```
serviceMap: {
  frontend: "https://8080-984c0a6f-3614-4dee-89f0-7d7cc03df6e9.koji-staging.com",
  backend: "https://3333-984c0a6f-3614-4dee-89f0-7d7cc03df6e9.koji-staging.com"}
```

The last step before testing our backend is to add the service to the build process. We do this by editing the `deploy.json` file.

Under the CODE section, look for the `.koji` folder and open it. Inside of the `project` folder, you should see the `deploy.json` file we are looking for.

Update it to look like this:

```
{
  "deploy": {
    "subdomain": ".withkoji.com",
    "frontend": {
      "output": "frontend/build",
      "commands": [
        "cd frontend",
        "npm install",
        "export NODE_ENV=production && npm run build"
      ]
    },
    "backend": {
      "output": "backend",
      "type": "dynamic",
      "commands": [
        "cd backend",
        "npm install",
        "export NODE_ENV=production && npm run compile"
      ]
    }
  }
}

```

You can see that this code looks very similar to our `develop.json` file, except that we will be running build commands instead of development commands.

### Confirming the serviceMap and server are working

Okay, so it _looks_ like everything is working. But we want to test it to make sure! We'll update our `frontend` to get our value from the server and display it automatically.

- Open up `/frontend/common/App.js` and replace the `App` component with the following code: 

```
class App extends React.Component {
    constructor(props) {
      super(props);

      this.state = { text: 'Updating in 5 seconds...' };
    }

    componentDidMount() {
      window.setTimeout(() => {
        fetch(Koji.config.serviceMap.backend)
          .then((response) => response.json())
          .then((jsonResponse) => {
            console.log('jsonResponse', jsonResponse);
            this.setState({ text: jsonResponse.text });
          });
        }, 5000);
    }

  render() {
    return (
      <Container>
        <h1>{Koji.config.strings.title}</h1>
        <p>{this.state.text}</p>
        <Icon />
      </Container>
    );
  }
}
```

We've set a default text state of "Updating in 5 seconds". After the component mounts, we will set a timeout of 5 seconds, and then try to fetch our backend route, and set the response as the new text state.

- Open up the preview window and refresh it. If everything worked, you should see the first text state for 5 seconds, and then it should be replaced with the "Hello, world!" text from the server!

The last step is to publish your app and confirm that the project is also working. Just publish the app as you normally would and you'll be able to check it yourself =)


#### Behind the scenes (Advanced/Debugging)

If you're wondering about how these things just seem to magically "happen", here is a brief explanation:

1. In your `develop.json` or `deploy.json` files inside `.koji/project`, there are service keys, like `frontend`
2. For each of these keys, the container that handles your editor (or published deployment) will look for those keys and automatically generate a URL
3. That url is written as an environment variable, based on the key. So for frontend, you will see an ENV like `KOJI_SERVICE_URL_FRONTEND`
4. For each of the ENV variables that match that pattern, the `@withkoij/vcc` package will expose those values in the `serviceMap`

### Wrapping up

Hopefully this has given you a better understanding of how to add a service, and the service map configuration.

Reach out to @diddy on [Discord](https://discord.gg/eQuMJF6) if you have questions or comments about this tutorial =)
