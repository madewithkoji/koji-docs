# Koji FAQ <!-- omit in toc -->
> This page exists to answer common questions about the Koji platform. It is not a complete guide, but is a quick reference to commonly repeated questions.
> If there is a common or important question you feel is missing here, please open a GitHub issue against this repository!

## Table of Contents <!-- omit in toc -->
- [How do I get started?](#how-do-i-get-started)
- [How do I start a new project?](#how-do-i-start-a-new-project)
- [How do I create a file?](#how-do-i-create-a-file)
- [How do I create a folder?](#how-do-i-create-a-folder)
- [How do upload a file?](#how-do-upload-a-file)
- [How can I download my code to my computer?](#how-can-i-download-my-code-to-my-computer)
- [How do I get VCC's into my app?](#how-do-i-get-vccs-into-my-app)
- [Live preview / detached window doesn't show my app](#live-preview--detached-window-doesnt-show-my-app)
  - [parcel example](#parcel-example)
- [Hows do I reset my project?](#hows-do-i-reset-my-project)
  - [Soft reset](#soft-reset)
  - [Hard reset](#hard-reset)
- [What are stubs?](#what-are-stubs)
- [Where did the developer portal go?](#where-did-the-developer-portal-go)

## How do I get started?
Checkout the [Getting Started WithKoji](http://bit.ly/StartWithKoji) tutorial video
️
> ☝
> You can also type `!newbie` or `!docs` in discord to get the tutorial video or docs sent to you.

## How do I start a new project?
1. Navigate to [withkoji.com/create](https://withkoji.com/create)
2. Click on an app you would like to use as a starter template.
3. Click on the **Remix** button to start working on your new project.

## How do I create a file?
In the project sidebar:
1. click on the ➕ icon across from the **code** section header or across from any of the project folders.
2. click **New file** and enter the path and file name of the file you want to create into the new file modal. eg `/myfile.txt`
3. click **Create file**

> ☝
> if type a file path that includes a directory that does not exist yet, the directory will be created for you. eg `/new_dir/new_file.txt`

You can also create a file using your terminal.  
```sh
touch my_new_file.txt
```

## How do I create a folder?

There is currently no feature for creating new folders in the editor. However, they can still be created by either:

1. Creating a nested file as explained above.

or

2. Use the terminal
```sh
mkdir my_new_dir
```

## How do upload a file?
The upload feature is currently not available.

However you can get files into your project in the terminal with either `git` or `wget`

git
```sh
git fetch remote_with_my_changes
```

wget
```sh
wget url_to_my_file
```

## How can I download my code to my computer?
There is no project download feature at the moment.

But since Koji uses git for version control, you can use git to clone your project to your local machine.

1. Run in your project terminal, to print your remote origin.
```sh
git ls-remote
```

2. On your local machine run.
```sh
git clone https://projects.koji-cdn.com/<project-hash>.git
```

Read more about project git repositories [here](https://github.com/madewithkoji/koji-docs/blob/master/koji-project-anatomy.md)

## How do I get VCC's into my app?
VCCs help no-code creators remix apps, they are just json files that the editor can create visual controls for. Although you can load them in however you want, we provide an npm package `koji-tools` for handling this conveniently.
```sh
npm install --save koji-tools
```

```js
// import
import Koji from 'koji-tools';

// load config
Koji.pageLoad();

// declare config
const config = Koji.config;

// handle vcc change
// update config
Koji.on('change', (scope, key, value) => {
    config[scope][key] = value;
});
```
for more info on how to use koji-tools, checkout the koji-tools [docs](https://github.com/madewithkoji/koji-tools/blob/master/README.md)

## Live preview / detached window doesn't show my app
Start/restart your development server.
1. In the terminal enter `Ctrl-C`
2. Restart your server with `npm start` or what ever your start command is.

Check that your `develop.json` file is configured correctly.
1. Navigate to `develop.json` from **Settings > Development**.
2. Verify paths, ports, and start commands are correct.
    1. path: the directory your package.json is located 
    2. port: the port your development server uses
3. Verify the event hook's matching values are partial matches for what the development server prints to standard out when each of those events happen.

> make sure there are no events without matching lines from standard out

### parcel example
standard out for running the `npm start` command.
```sh
$ npm start

$ hello-world-parcel@1.0.0 prestart hello-world-parcel
$ koji-tools watch &


$ hello-world-parcel@1.0.0 start hello-world-parcel
$ npx parcel index.html

koji-tools watching...
Server running at http://localhost:1234 
✨  Built in 1.92s.
```

develop.json
```js
{
  "develop": {
    "frontend": {
      "path": ".",
      "port": 1234,
      "events": {
        "started": "npm start",
        "building": "koji-tools watching",
        "built": "Built in",
        "build-error": "npm ERR!"
      },
      "startCommand": "npm start"
    }
  }
}
```

## Hows do I reset my project?
### Soft reset
Navigate to the development settings at **Settings > Development** and click on ***Force restart project*** in the **Actions** section.
### Hard reset
Navigate to the development settings at **Settings > Development** and click on ***Hard reset project*** in the **Actions** section. If the editor menu is unavailable you can also perform a hard reset by appending the `forceImageRebuild=true` parameter to the project url.

eg:
`https://withkoji.com/projects/<project-id>?forceImageRebuild=true`
> ⚠️ Performing a hard reset DELETES your Koji editor state and re-downloads from your project's remote repository. If you have changes that you have not committed and pushed to your remote repository THEY WILL BE LOST."

## What are stubs?
Stubs can be used to save time when starting a project. For example if you are starting a project with the same bundler as another project, you can use the other projects `develop.json` to get started quickly.

> ☝
> the link to your project's stub can be found under **Settings** > **Repository**

## Where did the developer portal go?
Koji is no longer split into gokoji.com and withkoji.com. Everything is merged on to withkoji.com

This means.
1. No more developer portal. Every published project is also a potential template.
2. There is no longer an approval process. Just publish your project and share it with everyone!

Watch the video announcement [here](https://youtu.be/Vub9epveeO0).