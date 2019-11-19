# Working Locally

Koji provides a feature-rich online project editor, but as a developer you are probably used to working with your own suite of development tools. In this tutorial, I will describe how to clone a Koji project template to your development machine, so that you can use it locally as the starting point for your own game.

## Required Tools

In order to develop your Koji project locally, you will need four tools:

1.  The open source **Git** version control system. Koji stores each template and each remixed project as a separate Git repository. To work on a project locally, you will need to use Git to download a clone of your project to your local hard-drive. You can then make changes and use Git to push the changes back to the `origin` repository on the Koji servers.  

    This tutorial assumes that you already have [Git](https://git-scm.com/downloads) installed on your computer, and that you are familiar with the basic Git commands. If you need to install Git on your computer, please do that first. You can find all the necessary information [here](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git).
2.  The open source **Node.js** server environment. Koji uses Node.js and many add-on packages to deliver your web app to your users' browsers. You will need to [install Node.js](https://nodejs.org/en/download/) and its associated Node Package Manager (NPM) in order to run a Node server on your development computer.

3.  A Terminal application. You will be typing commands that use `git` and `npm` to set up your development environment. Depending on your operating system, the application you use will be different. Here are some suggestions:

    *   [Terminal](https://blog.teamtreehouse.com/introduction-to-the-mac-os-x-command-line) on macOS
    *   [GitBash](https://msysgit.github.io) on Windows
    *   [Linux Terminal](https://www.howtogeek.com/140679/beginner-geek-how-to-start-using-the-linux-terminal/) on Linux
4.  A text editor. This is where you will edit your code, and save it as plain-text files. A text editor that is designed for writing code will help you in many ways. Computers do exactly what you tell them to do, even if you tell them the wrong things. A dedicated text editor will make it easier for you to find the right words to use, and to find where you have used the wrong words or the wrong structure. You can find a list of recommended text editors [here](https://kinsta.com/blog/best-text-editors/), including several which are both excellent and free.

# # # REMIXING YOUR FIRST PROJECT # # #

## Cloning the Git Repository

When you click on Remix (for any project), Koji creates a new clone of the project's Git repository just for you. This new repository will have a unique identifier string. If you clone the same project twice, you will get a different identifier string each time. The Koji repositories are stored at https://projects.koji-cdn.com/.

In order to clone the Koji repository locally, you will need three strings:

*   The URL of the repository on https://projects.koji-cdn.com/
*   The Username that you created when you registered with Koji
*   An Access Token, which you will need to generate and store securely.

You will need to use the repository URL only once, you should know your Username by heart, but the Access Token is a 32-character randomly-generated hexadecimal number, and you will need to make a note of it in a safe place.

### Repository URL

To find the URL of the Koji repository, open your project page and in the bottom left corner of the page, click on the disclose triangle beside the Settings icon to see a link to your Repository settings. Click on that to open the Repository Settings pane.

![Open the Repository Settings](working_locally/repository.png "Open the Repository Settings")

The Repository Settings pane will show links to two repositories. The first is called Remote Repository, and this is your personal repository. (The second, called the Upstream Repository, is the one your project was cloned from. You can ignore this... or create a fork of it if you want to suggest changes and make Pull Requests for it, so that other users of this template can benefit.)

Copy the Remote Repository URL, open a Terminal window, navigate to the directory where you want to save your project and type `git clone` followed by a space, and then paste the repository URL that you just copied. Press the Enter key. Your Terminal window might now look something like this:

<pre class="terminal"><span class="path">~/Repos/Koji$</span> **git clone https://projects.koji-cdn.com/<span class="custom">a70f8329-e89e-48b0-8d85-7658c1542b9f.git</span>**
Cloning into 'a70f8329-e89e-48b0-8d85-7658c1542b9f'...
Username for 'https://projects.koji-cdn.com':
</pre>

The Terminal is asking for your Username, and after that it will ask for a Password. This password is **not** the same as the password you use to log in to Koji. It is a strong machine-generated access key, which only you should ever know about.

### Access Key

To generate an Access Key, you need to visit the [Access key](https://withkoji.com/settings/access-keys) pane in your Settings page. There are two ways to find your Settings page:

1.  From your [Home page](https://withkoji.com/), press the Settings button at the bottom left
2.  From any page, press your avatar in the top right corner, and then on the Cog icon, as shown in the screenshot below.

When you are on your Settings page, press the [Access keys](https://withkoji.com/settings/access-keys) link in the column on the left.

![Open the Access Keys settings](working_locally/AccessToAccess.png "Open the Access Keys settings")

On the Access Key page, click on the Generate New Token link at the top right.

![Generate New Token](working_locally/GenerateToken.png "Generate a new token")

An overlay window will open containing a 128-bit number in hexadecimal form. Copy this number and keep it somewhere safe, where only you can find it easily. You will need to use it each time you clone, pull or push to the `origin` repository.

![Save your Access Key](working_locally/AccessKey.png "Save your Access Key")

After you close the overlay window, this number will be known only as `Token 1`, and you will have no way to retrieve it from the Koji site again. You can, however, revoke your token and generate a new one if you have forgotten it, or if you believe that someone unauthorized has discovered it.

### Username

Your username for the Koji Git repository is the same as the name you chose when you registered with Koji. For reference, it is shown at the bottom of the paragraph above your list of tokens.

### Authorizing Git to Clone the Repository

In the Terminal window, type in your Username, then press the Enter key. When asked for the password, paste in the 32-character hex string that you copied from the Access Key overlay window. (Remember than in some Terminal windows, `Ctrl-C` means "Cancel", so you may have to use `Shift-Ctrl-C` to paste). The password that you pasted will not be shown. Press the Enter key to start the cloning process.

<pre class="terminal"><span class="path">~/Repos/Koji$</span> git clone https://projects.koji-cdn.com/a70f8329-e89e-48b0-8d85-7658c1542b9f.git
Cloning into 'a70f8329-e89e-48b0-8d85-7658c1542b9f'...
**Username for 'https://projects.koji-cdn.com': <span class="custom">KojiCoder</span>
Password for 'https://KojiCoder@projects.koji-cdn.com':** 
remote: Counting objects: 15941, done.
remote: Compressing objects: 100% (6156/6156), done.
remote: Total 15941 (delta 9517), reused 15941 (delta 9517)
Receiving objects: 100% (15941/15941), 9.35 MiB | 754.00 KiB/s, done.
Resolving deltas: 100% (9517/9517), done.
Checking connectivity... done.
</pre>

## What You Get

You should now have a new directory with a beautifully arcane name like `a70f8329-e89e-48b0-8d85-7658c1542b9f`. For the rest of this tutorial, I'm going to imagine that you renamed it to `MyKojiGame`:

<pre class="terminal"><span class="path">~/Repos/Koji$</span> **mv <span class="custom">a70f8329-e89e-48b0-8d85-7658c1542b9f</span>/ MyKojiGame**
</pre>

Now you can `cd` into your `MyKojiGame` directory, and look at what has been cloned in:

<pre class="terminal"><span class="path">~/Repos/Koji$</span> **cd MyKojiGame/**
<span class="path">~/Repos/Koji$</span> **ls -al**
total 40
drwxrwxr-x  6 kojicoder dev 4096 nov  5 16:38 .
drwxrwxr-x 11 kojicoder dev 4096 nov  5 17:00 ..
drwxrwxr-x  3 kojicoder dev 4096 nov  5 16:38 backend
-rw-rw-r--  1 kojicoder dev  516 nov  5 16:38 Dockerfile
drwxrwxr-x  5 kojicoder dev 4096 nov  5 16:38 frontend
drwxrwxr-x  8 kojicoder dev 4096 nov  5 16:38 .git
-rw-rw-r--  1 kojicoder dev  186 nov  5 16:38 .gitignore
drwxrwxr-x  6 kojicoder dev 4096 nov  5 16:38 .koji
-rw-rw-r--  1 kojicoder dev   27 nov  5 16:38 package-lock.json
-rw-rw-r--  1 kojicoder dev  797 nov  5 16:38 README.md
</pre>
