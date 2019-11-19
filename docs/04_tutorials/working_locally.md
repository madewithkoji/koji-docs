

## Installing the Dependencies

The clone process installed all the project-specific files, but it only contains information about the servers and other dependencies that you will need to launch the game locally, but not the dependency files themselves. Before you can test the game on your development machine, you will need to install two sets of Node modules, one set to run the frontend and another set to run the backend.

To deal with the backend, `cd` to the `backend` director and run `npm install`. You'll have to wait for a minute or two while the Terminal window shows you what's happening.

<pre class="terminal"><span class="path">~/Repos/Koji/MyKojiGame$</span> **cd backend**
<span class="path">~/Repos/Koji/MyKojiGame/backend$</span> **npm install**

> core-js@2.6.9 postinstall /home/kojicoder/Repos/Koji/MyKojiGame/backend/node_modules/core-js
> node scripts/postinstall || echo "ignore"

<span class="npm">npm</span> <span class="warn">WARN</span> koji-project-backend@1.0.0 No description
<span class="npm">npm</span> <span class="warn">WARN</span> koji-project-backend@1.0.0 No repository field.
<span class="comment">... (more warnings and comments not shown) ...</span>

added 477 packages from 234 contributors and audited 8550 packages in 6.678s
found 0 vulnerabilities
</pre>

Dealing with the frontend is similar: `cd` to the `frontend` director and run `npm install`:

<pre class="terminal"><span class="path">~/Repos/Koji/MyKojiGame$</span> **cd ../frontend/**
<span class="path">~/Repos/Koji/MyKojiGame/frontend$</span> **npm install**

> koji-tools@0.5.3 postinstall /home/kojicoder/Repos/Koji/MyKojiGame/frontend/node_modules/koji-tools
> node ./cmd.js postinstall

new config

> preact@8.5.1 postinstall /home/kojicoder/Repos/Koji/MyKojiGame/frontend/node_modules/preact

> webpack-cli@3.3.1 postinstall /home/blackslate/Repos/Koji/MyKojiGame/frontend/node_modules/webpack-cli
> node ./bin/opencollective.js

<span class="npm">npm</span> <span class="warn">WARN</span> meta-project@1.0.0 No repository field.
<span class="npm">npm</span> <span class="warn">WARN</span> meta-project@1.0.0 No license field.
<span class="comment">... (more warnings and comments not shown) ...</span>

added 996 packages from 358 contributors and audited 12188 packages in 14.023s
found 345 vulnerabilities (1 low, 344 high)
  run `npm audit fix` to fix them, or `npm audit` for details
</pre>


#### Optional Audit

If you want to update all the Node modules to their most recent stable release, you can run `npm audit fix`.

<pre class="terminal"><span class="path">~/Repos/Koji/MyKojiGame/frontend$</span> **npm audit fix**
<span class="npm">npm</span> <span class="warn">WARN</span> meta-project@1.0.0 No repository field.
<span class="npm">npm</span> <span class="warn">WARN</span> meta-project@1.0.0 No license field.
<span class="comment">... (more warnings and comments not shown) ...</span>

removed 2 packages and updated 4 packages in 7.468s
fixed 344 of 345 vulnerabilities in 12188 scanned packages
  1 vulnerability required manual review and could not be updated
</pre>

This will alter the `package-lock.json` file, so that it can request the identical file versions if you run `npm install` on a different computer.


## Launching Your Project Locally

The Element Match game features both a frontend – so that you can play the game in your browser – and a backend – to access a database where the Leaderboard scores are stored.


#### Games With No Leaderboard

If you chose to develop a game that does **not** require a leaderboard, then launching your project locally is very simple. Simply `cd` to the `frontend` directory and run `npm start`:

<pre class="terminal"><span class="path">$</span> **cd ../frontend/**
<span class="path">$</span> **npm start**

<span class="comment">... (some output not shown) ...</span>
<span class="path">ℹ</span> <span class="wds">｢wds｣</span>: Project is running at <span class="path">[http://0.0.0.0:8080/](http://0.0.0.0:8080)</span>
<span class="path">ℹ</span> <span class="wds">｢wds｣</span>: webpack output is served from <span class="path">/</span>
<span class="comment">... (more output not shown) ...</span>

<span class="path">ℹ</span> <span class="wds">｢wds｣</span>: Compiled successfully
</pre>

You should now be able to visit [http://0.0.0.0:8080](http://0.0.0.0:8080) in your browser to test your project. Note that [http://localhost:8080/](http://localhost:8080/) and [http://127.0.0.1:8080/](http://127.0.0.1:8080/) might also work.


### Environment variables

If you want to run the Leaderboard, the procedure for launching your game is more complex. You will need to inform the frontend which URL to use to connect to the backend, and you will need to give the backend a couple of details concerning your project. And you will have to launch both the frontend and the backend servers on your local machine, using separate Terminal windows for each.

The Koji system reads in these items of information from [environment variables](https://en.wikipedia.org/wiki/Environment_variable). This ensures the data that is specific to deployment is kept separate from the code and configuration that is specific to your app.

To see what environment variables Koji uses to deploy your project, go to the Online Editor for your project, open a new Terminal tab at the foot the page and type:

    env | grep 'KOJI_'

Below is the output that I get, edited to show only the relevant items. The values that you will see in your output will be unique to your project.

<pre class="koji">root@ip-172-31-15-24:/usr/src/app# **env | grep 'KOJI_'**
<span class="comment">...</span>
KOJI_SERVICE_URL_backend=<span class="custom">https://3333-48006672-6558-4f69-a40c-e4142c15067f.koji-staging.com</span>

<span class="comment">... (some variables not shown) ...</span>

**KOJI_PROJECT_ID=<span class="custom">a70f8329-e89e-48b0-8d85-7658c1542b9f</span>
KOJI_PROJECT_TOKEN=<span class="custom">6679483a-dab8-4e89-9a83-6b56b53b4241</span>**
<span class="comment">...</span>
</pre>

### Launching the frontend

The backend is configured by default to run on port `3333`, while the frontend is configured to run at [http://0.0.0.0:8080](http://0.0.0.0:8080). When you launch the frontend, you need to tell it which URL it needs to use to access the backend. On Mac OS and other Unix-based operating systems, you can use the `export` command to do this.

Navigate to the `frontend` directory for your project, and then set the `KOJI_SERVICE_URL_backend` environment variable just before you call `npm start`. Your Terminal window might look something like this:

<pre class="terminal"><span class="path">$</span> **cd ..frontend/**
<span class="path">$</span> **export KOJI_SERVICE_URL_backend=http://0.0.0.0:3333 && npm start**

<span class="comment">... (some output not shown) ...</span>
<span class="path">ℹ</span> <span class="wds">｢wds｣</span>: Project is running at <span class="path">http://0.0.0.0:8080/</span>
<span class="path">ℹ</span> <span class="wds">｢wds｣</span>: webpack output is served from <span class="path">/</span>
<span class="comment">... (more output not shown) ...</span>

<span class="path">ℹ</span> <span class="wds">｢wds｣</span>: Compiled successfully
</pre>

### Launching the backend

The backend needs to have both a `KOJI_PROJECT_ID` and a `KOJI_PROJECT_TOKEN` in order to access the Leaderboard database. You can get the value of these variables from the Terminal tab in the Online Editor for your project. Type...

`env | grep 'KOJI_PROJECT_'`

... and copy the output that you get.

**Open a new Terminal window**, and navigate to the `backend` directory of your project. Now set these two variables just before you call `npm run start-dev`. Your Terminal window might look something like this:

<pre class="terminal"><span class="path">$</span> **cd ../backend/**
<span class="path">$</span> **export KOJI_PROJECT_ID=<span class="custom">a70f8329-e89e-48b0-8d85-7658c1542b9f</span> && export
KOJI_PROJECT_TOKEN=<span class="custom">6679483a-dab8-4e89-9a83-6b56b53b4241</span> && npm run start-dev**

> koji-project-backend@1.0.0 start-dev /home/kojicoder/Repos/Koji/Element Match/39ff/backend
> NODE_ENV=development babel-watch -L --watch ../.koji/ src/server.js

[koji] backend started
</pre>

Remember to use your own values in the place of the values shown in italics above.

## Testing Your Local Deployment

Check that the frontend reported `Compiled successfully` and that the backend reported `backend started`,. If you see errors, make sure that you have no other apps already running on port `8080` or port `3333`.

Now, in your browser, visit [http:0.0.0.0:8080](http:0.0.0.0:8080). You should see the Element Match game running.

Click on the Leaderboard button, to check that it is working. If you haven't played the game yet, there will be no scores to show, but you will still see the title "Top Scores" and a Close link that returns you to the Welcome screen.

Click on Start Matching, and start scoring points. When the game is over, you will be invited to submit your user name. This time, the Leaderboard should show your name and your score.
