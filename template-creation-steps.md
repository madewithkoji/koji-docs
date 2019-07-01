# Koji Templates
### What is a template?
Koji templates are fully completed apps that can be extended and changed easily on the Koji Platform.
### What are the requirements?
1. Koji apps are all javascript. Any framework can be used, but the template must be a web app.
2. Code is well written, documented, and editable.
3. Template is set up to be cross platform and responsive
\*full requirements are in our [sumbission guidelines document](https://docs.google.com/document/d/1CjXXS-XCd_hrusN0yUtwJvu5VnGWD8oNha88FqoSXcg/edit?usp=sharing).

## How to make a template

### Step 1: Pick a starting point
To create a template, you have 2 different options as starting points:

#### Start With a Koji Scaffold
![Scaffold Examples](https://i.imgur.com/M7PiaKs.png)
Koji Scaffolds have everything you need to develop in Koji already set up for you. They are available in many different javascript frameworks.

#### Start with an Existing Koji Template
![Existing Template Examples](https://i.imgur.com/CJyfZMb.png)
Take an existing template built by someone else and add on to it. Using this method it is important to make sure that the template is licensed to be extendable and that enough changes are going to be made to justify a new template. Examples of Koji templates from existing templates would be like adding new levels to an existing game or creating a new game mechanic to make an existing game into something else new and exciting.

Once you have found your starting point. Clone the template and begin your project.

### Step 2: Write your app
![Koji Editor Image](https://i.imgur.com/aeQoiOq.png)
 Every good template begins as a good Koji project. Use the Koji Editor to create a full web app.


### Step 3: Add and Check VCCs
![VCCs in Visual Editor](https://i.imgur.com/D3nMqnV.png)
VCCs are quick and easy customization options that are built into the Koji editor. They should be used for global properties across your app like images, game meta properties, or titles.
Do not use too many VCCs and turn your project into a template engine with unreadable spaghetti code.

You can add additional VCCs using the following format in the json files located in the `.koji/customization` directory:
```json
{
  "example":  {
    "param":  "this is the value of the param"
  },
  "@@editor":  [
    {
      "key":  "example",
      "name":  "Example",
      "icon":  "😄",
      "source":  "example.json",
      "fields":  [
        {
          "key":  "param",
          "name":  "An Example Parameter",
          "type":  "text"
        }
      ]
    }
  ]
}
```


### Step 4: Compatability and Responsiveness 
![Screen Sizes to Support](https://i.imgur.com/9vXqK6a.png)
A large part of a Koji template is cross platform support and responsiveness. Your template should look natural from a widescreen on desktop, a small screen from a laptop, a square tablet screen, and a narrow screen on a mobile phone. 
The way that your app is rendered on different screens will vary largely due to the content of your app. Use your best judgement when deciding what each screen will look like. 

### Step 5: Submitting your template
1. deploy your project and make sure that everything works on a deployed copy of your project.
2. Next, navigate to the Settings/Repository tab of the Project section in the left bar of your project.
![Repository tab](https://i.imgur.com/6pJ1HlE.png)
3. From that menu you should see a git link. Copy this link for later.    
4. Navigate to the developer portal at https://gokoji.com/developer and apply to be a template developer if you have not already.
5. Fill out the template submission form, using the git link you copied earlier as the git repository in the source code section.
### Step 6: Wait for approval
The Koji template store approval process takes at most 48 hours. Sit back and relax and wait for a response. 
\**Most of the time, small changes will be requested from templates to make sure your template will work for all people who want to use it.*
