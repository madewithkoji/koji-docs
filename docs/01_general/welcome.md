# Welcome to the Koji documentation

**Koji** is an app development platform that lets anyone create apps and games. Find something you like, remix it, and share it with the world.

The documentation is heavily work in progress. If you would like to contribute, or have 
questions or issues, visit the [GitHub Repository](https://github.com/madewithkoji/koji-docs). You can make a pull request to edit any documentation section, or even add new ones!

## Getting Started
Checkout the [Getting Started WithKoji](http://bit.ly/StartWithKoji) tutorial video on YouTube!
️
> ☝
> You can also type `!newbie` or `!docs` in Discord to get the tutorial video or docs sent to you.

## Starting a new project
1. Go to [withkoji.com/create](https://withkoji.com/create).
2. Click the app you would like to use as a starter template.
3. Click the **Remix** button to start working on your new project.

> If you want to start a fresh project, try any of the [blank starter templates](https://withkoji.com/search/blank%20starter).

## Importing existing projects
1. Hold `Shift+Alt+L` while in withkoji.com 
2. Click ***Create a new project from a remote repository*** in the Developer options.
3. Fill out the form. Enter a Project name, Git repository URL and Koji Stubs URL (optional). 
4. Click **Create project**.

> ⚠️
> Make sure you use the HTTPS address for the git repository.

## Creating a file

In the project sidebar:
1. Click the ➕ icon on the sidebar or while hovering over any folder.
2. Click **New file** and enter the path and file name of the file you want to create. ex: `/myfile.txt`
3. Click **Create file**

> ☝
> If your path includes a directory that does not exist, the directory will be created automatically. ex: `/new_dir/new_file.txt` (`new_dir` will be created for you).

You can also create a file using your terminal.  
```sh
touch my_new_file.txt
```

## Creating a folder

Currently, this is not possible. However, you can still create a folder by either:

1. Following the steps to create a file, but for the path you include the directory you want to be created.
2. Using the terminal
```sh
mkdir my_new_dir
```

## Uploading a file
Currently, this is not possible. However, you can add files to your project through the terminal using either `git` or `wget`

**git**:
```sh
git fetch remote_with_my_changes
```

**wget**:
```sh
wget url_to_my_file
```

## Downloading your files
Currently, this is not possible. However, as Koji uses `git` for version control, you can use `git` to clone your project locally.

1. Print your remote origin by running the following in your local terminal:
```sh
git ls-remote
```
2. Go to **Settings** -> **Development**.
3. Copy the **Remote Repository** URL, and then run:
```sh
git clone <URL>
```

[Read more about project git repositories here](https://github.com/madewithkoji/koji-docs/blob/master/koji-project-anatomy.md).

## Adding VCCs to your app
Visual Custom Controls help non-technical creators remix apps, they are just `JSON` files that the editor exposes to achieve this purpose. Although it is entirely possible for you to load them per your needs, we provide an npm package called `koji-tools` to handle this conveniently. 

1. First, install `koji-tools`.
```sh
npm install --save koji-tools
```
2. Then use koji-tools like the following:
```js
import Koji from 'koji-tools';

Koji.pageLoad();
const config = Koji.config;

// On any change, update the config
Koji.on('change', (scope, key, value) => {
    config[scope][key] = value;
});
```
For more info on how to use koji-tools, check out the [koji-tools github docs](https://github.com/madewithkoji/koji-tools/blob/master/README.md)
