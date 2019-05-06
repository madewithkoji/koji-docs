# Template Creation Process

1. Find some source files to paste in. In this tutorial I'm going to use The Coding Train's P5 flappy bird clone: https://github.com/CodingTrain/website/tree/master/CodingChallenges/CC_041_ClappyBird/P5
2. Go to gokoji.com and make an account if you don't already have one and go to templates and create a project from the *Koji P5 Scaffold* Template.
3. We now have a base Koji project, let's start copying over our repo. The first place that we're gonna do that is in index.html we need a couple p5 libraries. 
**`frontend/common/index.html`** in your `<head>`tag put the following:
```html
<script  language="javascript"  type="text/javascript"  src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/0.7.3/p5.min.js"></script>
<script  language="javascript"  src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/0.7.3/addons/p5.dom.min.js"></script>
<script  language="javascript"  src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/0.7.3/addons/p5.sound.min.js"></script>
```

4. Now `sketch.js` from the github repo will go into `frontend/pages/HomePage/index.js`
5. For simplicity we are going to paste `bird.js` and `pipe.js` into the same `frontend/pages/HomePage/index.js` right below what we just pasted.
6. Now we just need a couple more things in this file, at the top add:
```js
import Koji from 'koji-tools';
```
And at the bottom add:
```js
export default {
  setup,
  draw,
  keyPressed
};
```
That will tell Koji what P5 functions this file has.
Now the game should be showing up in the preview window. 

7. Now lets add some Koji Customization Controls. Near the top of the file you will see the `draw()` function, the first line of that function is `background(0);` change that to `background(Koji.config.colors.backgroundColor);` then go to the *Colors* tab on the left and try to change the background colors and it will update in real-time. You can create additional Customization options by editing the JSON files in the .koji directory and they will show up in the Customization menu. Access them in your code through `Koji.config`.
8. Add in Customization options to your hearts content. 
9. Go to the .git-info file and copy the *Remote URL* from the visual display that shows up. We will need it in a second.
10. Exit your project and go to the templates tab, thenfollow the 'open developer portal' link. You might have to agree to some TOS stuff to become a template creator.
11. Start a new Template from the Template Developer Page.
12. Add all of the relevent store listing information (image, name, description, tags, details)
13. On the next tab paste in the *Remote URL* that you copied a few steps ago into the *Git repository* field and leave *Koji stubs* blank. 
14. Complete the rest of the form and click *Submit*. After this your template will be reviewed by our team of developers and it will make its way into the store.
