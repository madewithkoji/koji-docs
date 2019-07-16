# Koji FAQ <!-- omit in toc -->
> This page exists to answer common questions about the Koji platform. It is not a complete guide, but is a quick reference to commonly repeated questions.
> If there is a common or important question you feel is missing here, please open a GitHub issue against this repository!

## Table of Contents <!-- omit in toc -->
- [How do I get started?](#How-do-I-get-started)
- [How do I start a new project?](#How-do-I-start-a-new-project)
- [How can I download my code to my computer?](#How-can-I-download-my-code-to-my-computer)
- [How do I create a folder?](#How-do-I-create-a-folder)
- [How do I get VCC's into my app?](#How-do-I-get-VCCs-into-my-app)
- [What are stubs?](#What-are-stubs)
- [Where did the developer portal go?](#Where-did-the-developer-portal-go)

## How do I get started?
Checkout the [Getting Started WithKoji tutorial video](http://bit.ly/StartWithKoji)
️
> ☝
> You can also type `!newbie` or `!docs` in discord to get the tutorial video or docs sent to you.

## How do I start a new project?
Navigate to [withkoji.com/create](https://withkoji.com/create) and click on an app you would like to use as a starter template and click on the **Remix** button to start working on your new project.

## How can I download my code to my computer?
There is no project download feature at the moment. However, Koji uses git for version control so you can clone your project to your local machine using an **Access Token** that you can generate in your **Account Settings**

## How do I create a folder?
Right now the easiest way to make a folder is to use the plus button in the left side file menu in a project to bring up the new file prompt. If a file is added into a directory that does not yet exist, Koji will create that directory.
<!-- ![Create a new directory from file prompt](https://i.imgur.com/ZzYtwNb.png) -->

## How do I get VCC's into my app?
VCCs help no-code creators remix apps, they are just json files that the editor can provide visual controls for and you can load them in however you want. however, we have a package npm package `koji-tools` that handles this conveniently.
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