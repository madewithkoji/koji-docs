

## Smoke Test

Now you have a template project running on your local computer, so you can use your own favourite text editor to modify it and add new features. You will also be able to test that these new features work on your development machine. However, in order to share your project with the world, you need to get your updated project running on the Koji servers.

Before you get deep into development locally, it would be a good idea to make just one minor change, push it back to the Koji servers, and check that the change is visible to the world.

### Changing a Visual Customization Control

The simplest change that will be the most immediately obvious is the title of the game. To change this in the Online Editor, the easiest way is to click on the Customization > Game Settings link in the column on the left. This will open the Game Settings editor in Visual Mode. This is the way the non-coding _makers_ will customize your game when you have finished developing it.

However, Visual Mode is not available when you are developing your project locally. You will need to open the file at `.koji/customization/string.json` in your favorite text editor, and make the change there:

<pre class="editor">
{
  "strings": {
    "fontFamily": "https://fonts.googleapis.com/css?family=Lilita+One",
    "title": "<span class="hilite">CUSTOM TITLE</span>",
    "instructions1": "Swipe the Elements to move them",
    <span class="comment">... (more json code here) ...</span>

</pre>

Save your file with your new title, then in your browser at [http://0.0.0.0:8080](http://0.0.0.0:8080), check that the splash screen now shows the new title.

## From Local to Live

Making this change visible on Koji server is a four-step process.

1.  Tell Git to `commit` your changes locally
2.  Tell Git to `push` the committed changes to the remote Koji repository
3.  Tell the instance of Git in Koji's Online Editor to `pull` the commited changes from the Koji repository
4.  Tell the Koji Editor to Publish the updated version of your game.

### Tracking Changes with Git

Each time you make a change to one of the files in your project, the Git version control system will note that the file has changed. You can use the command `git status` to discover which files have changed.


#### Note

At the root of your project, there is a file called `.gitignore`. (You might not see it because, by default, files whose names begin with a dot are hidden by your operating system.)

The `.gitignore` file tells Git which files and folders to ignore. In particular, this file tells Git to ignore any changes made to files in the `node_modules` folders. These are the folders that were created when you ran `npm install` from within the `frontend/` and `backend/` folders. However, if you make a change to any file that is not an a `node_modules` folder, Git will be aware of your changes.


When you are ready to test your changes on the Koji server, open a Terminal window into the root directory of your project (`MyKojiGame/`, in my case) and run the following commands:

<pre class="terminal"><span class="path">$</span> **git add .                      **<span class="comment"> # tells Git to process all the documents that you have changed</span>
<span class="path">$</span> **git commit -m "<span class="custom">(your comments)</span>"** <span class="comment"># tells Git to create an updated version of your project</span>
</pre>

Note that you should replace `(your comments)` with a brief description of the changes that you have made, so that you will be able to remember what features you were working on.


#### Understanding Git `commit`

You can find a more complete description of the commands `git add` and `git commit` commands [here](https://www.atlassian.com/git/tutorials/saving-changes).
### Pushing Changes to the Koji Repository

After the `commit` process is successfully completed, you can use Git to upload the changes you have made to the repository stored on the Koji servers. To do this, you need to use the command `git push origin master`. You will be asked for your Koji username, and for your password. Your password is the Access Key that you created earlier and stored somewhere safely.

<pre class="terminal"><span class="path">$</span> **git push origin master** <span class="comment"># tells Git to upload your new version to the Koji repository</span>
Counting objects: 21, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (21/21), done.
Writing objects: 100% (21/21), 84.53 KiB | 0 bytes/s, done.
Total 21 (delta 15), reused 0 (delta 0)
**Username for 'https://projects.koji-cdn.com': <span class="custom">KojiCoder</span>
Password for 'https://blackslate@projects.koji-cdn.com':** <span class="comment">Paste your 32-character Access Key here</span>
To https://projects.koji-cdn.com/a70f8329-e89e-48b0-8d85-7658c1542b9f.git
   a88036c..ea6bda1  master -> master
</pre>

## Testing Your Updated App on the Koji Server

If you test your game in the Online Editor on the Koji Server now, you will not see any changes. This is because the Online Editor uses its own repository, which is different from the `origin` repository that you have just pushed your changes to. In order to update the repository used by the Online Editor, you will need to `pull` them from the `origin` repository.

To do this, you can use the command `git pull origin master`.

<pre class="koji">root@ip-172-31-12-226:/usr/src/app# **git pull origin master**
remote: Counting objects: 21, done.
remote: Compressing objects: 100% (21/21), done.
remote: Total 21 (delta 15), reused 0 (delta 0)
Unpacking objects: 100% (21/21), done.
From https://projects.koji-cdn.com/a70f8329-e89e-48b0-8d85-7658c1542b9f
 * branch            master     -> FETCH_HEAD
   a88036c..ea6bda1  master     -> origin/master
Updating a88036c..ea6bda1
Fast-forward
 backend/package-lock.json        | 41 <span class="green">++++++++++++++++++++++++++++++</span><span class="red">-----------</span>
 frontend/package-lock.json       | 82 <span class="green">+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++</span><span class="red">---------------------</span>
 .koji/customization/strings.json |  2 <span class="green">+</span><span class="red">-</span>
 3 files changed, 92 insertions(+), 33 deletions(-)
</pre>

### Previewing Your Modified App

The code in the Online Editor should now be identical to the code in your local repository. In the Online Editor, in the Preview pane on the right, you should now see your custom title, corresponding to the change you made in `.koji/customization/strings.json`.

### Updating the Node Modules

Your Git repository does not store any of the files in the `node_modules` directory, and indeed, the Online Editor does not even display this folder. However, if you ran `npm audit fix` earlier, the `package.json` and `[package-lock.json](https://medium.com/coinmonks/everything-you-wanted-to-know-about-package-lock-json-b81911aa8ab8)` files in both the `backend` and the `frontend` directories may have changed, so you do at least have the information that you need to update your node modules to their most recent version. To benefit from this, you'll need to run `npm install` for both the `frontend` and the `backend` in the Terminal pane of the Online Editor.

1.  Click on the `frontend` tab of the Terminal pane
2.  Press `Ctrl-C` on your keyboard to halt the frontend server
3.  Run `npm install`
4.  Wait for the newest version of each module to be installed, then run `npm start`

<pre class="koji">**^C**
root@ip-172-31-15-216:/usr/src/app/frontend# **npm install**
<span class="npm">npm</span> <span class="warn">WARN</span> meta-project@1.0.0 No repository field.
<span class="npm">npm</span> <span class="warn">WARN</span> meta-project@1.0.0 No license field.
<span class="comment">... (more warnings and comments not shown) ...</span>

audited 12334 packages in 5.192s
found 1 low severity vulnerability
  run `npm audit fix` to fix them, or `npm audit` for details
root@ip-172-31-15-216:/usr/src/app/frontend# **npm start**
<span class="comment">... (more output not shown) ...</span>

<span class="path">ℹ</span> <span class="wds">｢wds｣</span>: Compiled successfully
</pre>

1.  Click on the `backend` tab of the Terminal pane
2.  Press `Ctrl-C` on your keyboard to halt the backend server
3.  Run `npm install`
4.  Wait for any new modules to be installed, then run `npm run start-dev`

<pre class="koji">**^C**
root@ip-172-31-15-216:/usr/src/app/backend# **npm install**
<span class="npm">npm</span> <span class="warn">WARN</span> koji-project-backend@1.0.0 No description
<span class="npm">npm</span> <span class="warn">WARN</span> koji-project-backend@1.0.0 No repository field.
<span class="comment">... (more warnings and comments not shown) ...</span>

audited 8550 packages in 2.729s
found 0 vulnerabilities

root@ip-172-31-15-216:/usr/src/app/backend# **npm run start-dev**
<span class="comment">... (more output not shown) ...</span>

[koji] backend started
</pre>

### Publishing Your New Version

To complete the first iteration cycle of your new project, you now need to publish it, so that you can test how it behaves when served live from the Koji servers. You have only just started development, so you probably don't want its existence broadcast to the whole of the World Wide Web quite yet. For now, being able to test your game live yourself, or with feedback from a small hand-picked group, is good enough. Fortunately, Koji gives you a way to publish your project _unlisted_. This means that only people who obtain the direct URL will be able to visit your published app.

In the Online Editor:

1.  Click on the Publish Now link near the top of the left-hand column.
2.  Open the Discovery Settings (Optional) disclose triangle, near the middle of the page, to show more options
3.  Ensure that the Unlisted checkbox is selected
4.  Make any other changes that you want in the General Information Zone
5.  Click on Publish New Version.

![Publishing an Unlisted project](working_locally/Publish.png "Publishing an Unlisted project")

You will have to wait a few moments while your project is published, then you will be able to click on the Open Published App link near the top of the left-hand column, to test your game live.

![Publishing an Unlisted project](working_locally/live.png "Publishing an Unlisted project")


#### Getting Listed

By default, from now on, each time you publish your project, it will be published _unlisted_. When your project is ready for the world to see, don't forget to remove this setting.



#### Tip: Using a Separate Koji Project to Edit VCC Files Online

The Online Editor provides two features that are not available in your local development environment:

*   A Visual Mode to edit the JSON files stored at `.koji/customization/`
*   The ability to generate custom URLs for customizable assets

If you edit the JSON files stored at `.koji/customization/` in your local development environment, you risk creating valid JSON that nonetheless fails to conform to the expectations of the Online Editor. In particular, the `"@@editor"` array needs to contain specific property-value pairs and precisely constructed objects, otherwise the Visual Mode will fail to work, and makers will not be able to customize your game correctly. In order to ensure that your changes to the `"@@editor"` array are valid, it makes sense to work in the Online Editor, and to toggle back and forth between the Visual and Code modes, checking as you go that the Visual Mode works the way you expect it to.

Another reason for using the VCC editor online in Visual Mode is that you can upload images and audio files, or provide a direct URL to where these files can be found online, and the Koji system will copy them to the Koji CDN servers and insert the appropriate URL into the associated JSON file for you. Working in your local development environment, there is no way for you to transfer files to the Koji servers and to obtain their URLs.

But there is a down side to this. If you edited these customization JSON files in the Online Editor or used the Online Editor in Visual Mode to link to different assets, then you would need to manually `push` the changes to Koji's `origin` repository and then `pull` them into your local development environment. And if you had also made local changes since your last local commit, this might result in conflicts between the Online Editor's repository and your local Git repository.

In other words, in either of these two cases, editing the JSON files stored at `.koji/customization/` could lead to problems. Editing in the Online Editor could lead to one kind of problem, and editing locally could lead to another.

Fortunately, there is a simple solution:

1.  Create a separate Koji project _specifically for editing VCC JSON files, and nothing else_
2.  For each of the JSON files stored in the `.koji/customization/` folder of this separate project, edit the `"@@editor"` array until the Visual Mode works the way you want it
3.  Use the customized Visual Mode in this separate project to set the VCC values that you want in your main project
4.  When you have all the customized values you need in the Online Editor, switch to Code Mode and copy the entire JSON content
5.  In your local development environment, paste this tried and tested VCC code into the appropriate JSON file
6.  Commit your changes locally, push to the Koji `origin` repository, and pull the changes into the the Online Editor of your main project.

This way, you can be sure your modified code always flows only in one direction: _from_ your local development environment _to_ the online Koji environment.

The separate Koji project that you use for editing and testing the VCC JSON files can be reduced to its bare bones, if you want. You can delete all the frontend and backend code, and retain just the `.koji` directory and its contents. You can use the same `.koji` directory to store all the VCC JSON files for all your projects. It's just a container that lets you take advantage of the online-only Visual Mode.


## Conclusion

This tutorial has taken you on a round trip from the Koji Online Editor to your local development environment and back again. You have seen changes that you made locally served live from the Koji servers.

As you develop your project, you will cycle through many such loops, adding and refining features and testing that everything works just as beautifully when delivered from the Koji servers as from the comfort of your own development machine.

In particular, you have seen how to:

*   Find the required tools: Git, Node.js, a Terminal application and a text editor
*   Clone the Git repository for your project onto your local machine
*   Find the username and password that allows you to interact with Koji's `origin` repository
*   Install the Node modules that are needed to run a server on your local machine
*   Make and test changes locally
*   Push your changes to the `origin` repository, and then pull them into the Online Editor
*   Publish your changes to the Koji server
*   Test that your game behaves the same live on a Koji server as it does locally
*   Take full advantage of the Visual Mode for the JSON customization files, so that makers will have an easy job of making their own versions of your game.

You're now ready to start developing your Koji game in earnest, in the development environment where you feel most comfortable. Let's see your creativity shine!
