## Example Template: Connect Zoo
![The Finished Result](https://i.imgur.com/3gkG9wx.png)    
In this walkthrough we will be looking at the steps required to publish the template [Connect Zoo](https://gokoji.com/templates/2f014875-f71f-4763-87f7-8231227bd786).

This is a simple game written in [p5.js](https://p5js.org/) where you must drag to connect multiple of the same animal that are close together in order to get points.

### Step 1: Pick a starting point
This step is simple. We are using p5.js so we are going to use the [Koji p5.js Scaffold](https://gokoji.com/templates/9bf5122f-8fc2-49ce-8a28-a3bcbe7a3981).
![P5.js scaffold image](https://i.imgur.com/VCf0UXz.png)     


### Step 2: Write your app
Now that we are in the Koji Editor with a project created from the Koji p5.js scaffold, we can begin to write code.
We will be mainly using one file, `/frontend/app/index.js` which we will write our p5 code in.

![Where to find index.js](https://i.imgur.com/albnNwO.png)     
The bulk of the code for the template will go into this file. It can be found at the [Github repo for Connect Zoo](https://github.com/madewithkoji/connect-zoo/blob/master/frontend/app/index.js).

### Step 3: Add and Check VCCs
A Koji template wouldn't be complete without VCCs. ([What are VCCs?](https://github.com/madewithkoji/koji-docs/blob/master/koji-faq.md#what-is-a-vcc))    
For deciding what should be customizable remember the *VCC Rule of Thumb:*
**Any constant variable that either references an external resource or describes a global property of your template.**
Finding which variables should be VCCs is pretty simple for this template. 
Things that are VCCs:
- The 6 different zoo animals that you link together (images)
- Our background image (images)
- The color for text that is placed on the screen. (colors)
- The backup background color. (colors)
- Two strings that are in a popup asking the user to install the app. (strings)

After finding what all of our VCCs and the category they are in, let's implement them. We need a different 

We can separate all of our VCCs into three different files based on what type of content they describe (`image`, `color`, and `text`).

For example, we will look at the `strings.json` file which is located at `/.koji/customization/strings.json`.

```json
{
  "colors": {
    "backgroundColor": "#e2e5ec",
    "textColor": "#131313"
  },
  "@@editor": [
    {
      "key": "colors",
      "name": "Colors",
      "icon": "💅",
      "source": "colors.json",
      "fields": [
        {
          "key": "textColor",
          "name": "Text color",
          "description": "Default color for text",
          "type": "color"
        },
        {
          "key": "backgroundColor",
          "name": "Background color",
          "type": "color"
        }
      ]
    }
  ]
```
With the `koji-tools` package up and running, we reference these VCCs using `Koji.config`. For example, printing `textColor` to the screen would look like:
```js
console.log(Koji.config.colors.textColor);
```
Where `colors` is the key of the config file, and `textColor` is key of the CVV with the color we are looking for.

### Step 4: Compatability and Responsiveness 
For this step, I have a couple different browsers and use a friend's phone to test across as many operating systems and screen sizes as possible. 



### Step 5: Submitting your template
![Where is repository.](https://i.imgur.com/JLSWcrT.png)     
First we go to Settings/Repository to find the git link for our project, we will use that later in submitting the template.

Then we navigate to gokoji.com/developer and create a new template submission.
![Template submission](https://i.imgur.com/QQgzUGc.png)     
After filling out all of the metadata, we go to the next page and paste in the link from the beginning of this step and leave the 'stubs' section blank.
![Git repo paste](https://i.imgur.com/bptbOVg.png)     
Finally, we finish the rest of the steps and submit the template for review.

### Step 6: Wait for approval
After waiting at most 48 hours. Our template is live in the store and ready to use. the developer page tracks statistics on how many times the template has been viewed and cloned.
￼![Stats Page](https://i.imgur.com/9Demxw0.png)     

### Success!
