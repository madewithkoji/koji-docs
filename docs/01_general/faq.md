# Koji FAQ

This page exists to answer common questions about the Koji platform. It is not a complete guide, but is a quick reference to commonly repeated questions. If there is a common or important question you feel is missing here, please open a GitHub issue against this repository!

## Why doesn't live preview / detached window show my app?

To fix this, you can: 

**Restart your development server.**
1. Hold `CTRL+C` while in the terminal, this will kill the currently running process.
2. Restart your server with `npm start` or the respective start command that is defined in your scripts.

**Check that your `develop.json` file is configured correctly.**
1. Navigate to `develop.json` from **Settings > Development**.
2. Verify paths, ports, and start commands are correct.
    1. `path`: The directory your `package.json` is located in.
    2. `port`: The port your development server uses.
3. Verify the event hook's matching values are partial matches for what the development server prints to `std::out` when any one of those events fire.

> Please ensure that there aren't any events that print inconsistent messages to `std::out`.

**A good `std::out` should look like this:**

```bash
$ npm start

$ hello-world@1.0.0 prestart hello-world
$ koji-tools watch &


$ hello-world@1.0.0 start hello-world
$ npx parcel index.html

koji-tools watching...
Server running at http://localhost:1234 
✨  Built in 1.92s.
```
**A good `develop.json` should look like this:**

```json
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

## How do I reset my project?

### Soft reset
Navigate to the development settings at **Settings > Development** and click on ***Force restart project*** in the **Actions** section.

### Hard reset
Navigate to the development settings at **Settings > Development** and click on ***Hard reset project*** in the **Actions** section. If the editor menu is unavailable you can also perform a hard reset by appending the `forceImageRebuild=true` parameter to the project url.

Example:
`https://withkoji.com/projects/<project-id>?forceImageRebuild=true`
> ⚠️
> Performing a hard reset DELETES your Koji editor state and re-downloads from your project's remote repository. If you have changes that you have not committed and pushed to your remote repository THEY WILL BE LOST."

## How do I get the latest version of my client?
To make sure you have the latest client, hold the following key combinations in your respective browser:

**Chrome**: `Ctrl + F5` (Linux/Windows) or `Cmd + Shift + R` (Mac).

**Firefox**: `Ctrl + Shift + R` (Linux/Windows) or `Cmd + Shift + R` (Mac).

**Safari**: Hold the `hift` key, then click `Reload` in the toolbar.

**Edge**: `Ctrl + F5`.

See full instructions for [hard refresh](https://en.wikipedia.org/wiki/Wikipedia:Bypass_your_cache#Bypassing_cache)
or [clear cache](https://en.wikipedia.org/wiki/Wikipedia:Bypass_your_cache#Cache_clearing_and_disabling).

## Where did the developer portal go?
Koji is no longer split into gokoji.com and withkoji.com. Everything has been merged in to withkoji.com

This means:
1. No more developer portal. Every published project is also a potential template.
2. There is no longer an approval process. Just publish your project and share it with everyone!

Watch the video announcement [here](https://youtu.be/Vub9epveeO0).

## What are stubs?
Stubs can be used to save time when starting a project. For example if you are starting a project with the same bundler as another project, you can use the other projects `develop.json` to get started quickly.

> ☝
> The link to your project's stub can be found under **Settings** > **Repository**
