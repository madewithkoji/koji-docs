# Working Locally

Koji provides a feature-rich online project editor, but as a developer you are probably used to working with your own suite of development tools. In the [previous tutorial](using_git_to_manage_your_project.md), I described how to use Git to clone a Koji project template to your development machine, so that you can work on it locally to build your own game. If you haven't done so yet [go back](using_git_to_manage_your_project.md) and clone your project locally.

This is the second section in a three-part tutorial:

1. [Using Git to Manage Your Project](using_git_to_manage_your_project.md)
2. Working Locally (this article)
3. [Publishing the Project You Developed Locally](publishing_your_local_project.md)

In this section, you will be learning two key skills:

*  How to install a Node.js web server environment on your development machine
*  How to launch both a frontend and a backend server so that you can play your game and view the leaderboard, all within your local area network.

## Required Tools

In this section, you will be setting up a Node.js server on your development computer. Koji uses Node.js and many add-on packages to deliver your web app to your users' browsers. If you haven't already done so, you will need to [install Node.js](https://nodejs.org/en/download/) and its associated Node Package Manager (NPM).


## Installing the Dependencies

In the [previous tutorial](using_git_to_manage_your_project.md), you used Git to clone all the project-specific files to your local drive. However, the files that actually run the a local server have not yet been installed. Fortunately, amongst the files that were cloned are four files that contain all the information about the servers and other dependencies that you will need to set up the server environment and launch the game locally. These are the `package.json` and `package-lock.json` files that you can find in the `frontend` and `backend` directories.

Before you can test the game on your development machine, you will need to install two sets of Node modules, one set to run the frontend and another set to run the backend.

To deal with the backend, `cd` to the `backend` director and run `npm install`. NPM will read the contents of the `package-lock.json` file and install all the files that are listed inside a `node_modules` directory inside the `backend` directory. You'll have to wait for a minute or two while the Terminal window shows you what's happening.

```bash
~/Repos/Koji/MyKojiGame$ cd backend
~/Repos/Koji/MyKojiGame/backend$ npm install

> core-js@2.6.9 postinstall /home/kojicoder/Repos/Koji/MyKojiGame/backend/node_modules/core-js
> node scripts/postinstall || echo "ignore"

npm WARN koji-project-backend@1.0.0 No description
npm WARN koji-project-backend@1.0.0 No repository field.
... (more warnings and comments not shown) ...

added 477 packages from 234 contributors and audited 8550 packages in 6.678s
found 0 vulnerabilities
```

Dealing with the frontend is similar: `cd` to the `frontend` director and run `npm install`:

```bash
~/Repos/Koji/MyKojiGame$ cd ../frontend/
~/Repos/Koji/MyKojiGame/frontend$ npm install

> koji-tools@0.5.3 postinstall /home/kojicoder/Repos/Koji/MyKojiGame/frontend/node_modules/koji-tools
> node ./cmd.js postinstall

new config

> preact@8.5.1 postinstall /home/kojicoder/Repos/Koji/MyKojiGame/frontend/node_modules/preact

> webpack-cli@3.3.1 postinstall /home/blackslate/Repos/Koji/MyKojiGame/frontend/node_modules/webpack-cli
> node ./bin/opencollective.js

npm WARN  meta-project@1.0.0 No repository field.
npm WARN meta-project@1.0.0 No license field.
... (more warnings and comments not shown) ...

added 996 packages from 358 contributors and audited 12188 packages in 14.023s
found 345 vulnerabilities (1 low, 344 high)
  run `npm audit fix` to fix them, or `npm audit` for details
```

#### Optional Audit

If you want to update all the Node modules to their most recent stable release, you can run `npm audit fix`.

```bash
~/Repos/Koji/MyKojiGame/frontend$ npm audit fix
npm WARN meta-project@1.0.0 No repository field.
npm WARN meta-project@1.0.0 No license field.
... (more warnings and comments not shown) ...

removed 2 packages and updated 4 packages in 7.468s
fixed 344 of 345 vulnerabilities in 12188 scanned packages
  1 vulnerability required manual review and could not be updated
```

This will alter the `package-lock.json` file, so that it can request the identical file versions if you run `npm install` on a different computer.

## Launching Your Project Locally

The Element Match game features both a frontend – so that you can play the game in your browser – and a backend – to access a database where the Leaderboard scores are stored.


#### Games With No Leaderboard

If you chose to develop a game that does **not** require a leaderboard, then launching your project locally is very simple. Simply `cd` to the `frontend` directory and run `npm start`:

```bash
$ cd frontend/
$ nmp install
$ npm start

... (some output not shown) ...
ℹ ｢wds｣: Project is running at http://0.0.0.0:8080/
ℹ ｢wds｣: webpack output is served from /
... (more output not shown) ...

ℹ ｢wds｣: Compiled successfully
```

You should now be able to visit [http://0.0.0.0:8080](http://0.0.0.0:8080) in your browser to test your project. Note that [http://localhost:8080/](http://localhost:8080/) and [http://127.0.0.1:8080/](http://127.0.0.1:8080/) might also work.


### Environment variables

If you want to run the Leaderboard, the procedure for launching your game is more complex. You will need to inform the frontend which URL to use to connect to the backend, and you will need to give the backend a couple of details concerning your project. And you will have to launch both the frontend and the backend servers on your local machine, using separate Terminal windows for each.

The Koji system reads in these details from [environment variables](https://en.wikipedia.org/wiki/Environment_variable). This ensures the data that is specific to deployment is kept separate from the code and configuration that is specific to your app.

To see what environment variables Koji uses to deploy your project, go to the Online Editor for your project, open a new Terminal tab at the foot the page and type:

```bash
env | grep 'KOJI_'
```

Below is the output that I get, edited to show only the relevant items. The values that you will see in your output will be unique to your project.

```bash
root@ip-172-31-15-24:/usr/src/app# env | grep 'KOJI_'
...
KOJI_SERVICE_URL_backend=https://3333-48006672-6558-4f69-a40c-e4142c15067f.koji-staging.com

... (some variables not shown) ...

KOJI_PROJECT_ID=a70f8329-e89e-48b0-8d85-7658c1542b9f
KOJI_PROJECT_TOKEN=6679483a-dab8-4e89-9a83-6b56b53b4241
...
```

### Launching the frontend

The backend is configured by default to run on port `3333`, while the frontend is configured to run at [http://0.0.0.0:8080](http://0.0.0.0:8080). When you launch the frontend, you need to tell it which URL it needs to use to access the backend. On Mac OS and other Unix-based operating systems, you can use the `export` command to do this.

Navigate to the `frontend` directory for your project, and then set the `KOJI_SERVICE_URL_backend` environment variable just before you call `npm start`. Your Terminal window might look something like this:

```bash
$ cd ..frontend/
$ export KOJI_SERVICE_URL_backend=http://0.0.0.0:3333 && npm start

... (some output not shown) ...
ℹ ｢wds｣: Project is running at http://0.0.0.0:8080/
ℹ ｢wds｣: webpack output is served from /
... (more output not shown) ...

ℹ ｢wds｣: Compiled successfully
```

### Launching the backend

The backend needs to have both a `KOJI_PROJECT_ID` and a `KOJI_PROJECT_TOKEN` in order to access the Leaderboard database. You can get the value of these variables from the Terminal tab in the Online Editor for your project. Type...

```bash
env | grep 'KOJI_PROJECT_'
```

... and copy the output that you get.

Create a file named `.env` at the root of your project, and paste the two lines that you have just copied into it. Your file might look something like this (but your values will be unique to you):

```bash
KOJI_PROJECT_ID=c00484db-827a-45bb-8541-f2c09c2f192e
KOJI_PROJECT_TOKEN=a6676f53-44fe-4109-819a-69df620ad7ed
```
Save your new file.

**Open a new Terminal window**, navigate to the `backend` directory of your project, and run the command `npm run start-dev`. Your Terminal window might look something like this:

```bash
$ cd ../backend/
$ npm run start-dev

> koji-project-backend@1.0.0 start-dev /home/kojicoder/Repos/Koji/MyKojiGame/backend
> NODE_ENV=development babel-watch -L --watch ../.koji/ src/server.js

[koji] backend started
```
Before `npm` starts the backend Node.js server, it will read the values that you entered into the `.env` file into the environment variables, so the backend server will know how to contact the Koji database.

**Note:** The Koji database is not running on your local machine, so you will still need an active Internet connection in order to get the Leaderboard to work. However, you will not have any changes to make to the Koji database system, so you can focus on developing your game.

## Testing Your Local Deployment

Check that the frontend reported `Compiled successfully` and that the backend reported `backend started`,. If you see errors, make sure that you have no other apps already running on port `8080` or port `3333`.

Now, in your browser, visit [http:0.0.0.0:8080](http:0.0.0.0:8080). You should see the Element Match game running.

Click on the Leaderboard button, to check that it is working. If you haven't played the game yet, there will be no scores to show, but you will still see the title "Top Scores" and a Close link that returns you to the Welcome screen.

Click on Start Matching, and start scoring points. When the game is over, you will be invited to submit your user name. This time, the Leaderboard should show your name and your score.

## The Story So Far

In this section of the tutorial, you have seen how to:

1. Install the Node modules that are needed to run a server on your local machine
2. Find the environment variables used by the server in Koji's online environment
3. Create an `.env` file to apply these environment variables to the backend server, so that it knows how to connect to the database
4. Tell the frontend which URL to use to find the backend server
5. Launch the scaffold project
6. Check that the game and the leaderboard are working correctly

You are now ready to start editing this scaffold and your development machine and to turn this project into your own game. With what you have learned so far, you will be able to test all aspects of your game locally in your browser. But your future end-users will not be able to access your game from your development machine; they can only access it from the Koji server. In order to complete the development process, you will need to update the repository that the Koji server uses to deliver your game. That is the topic of the next section in this tutorial: [Publishing the Project You Developed Locally](publishing_your_local_project.md).
