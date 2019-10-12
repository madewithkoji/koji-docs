# VCC Reference
## Visual customization controls

- VCC's are Visual Customization Controls that allow another person who will remix your project to easily modify a game or app to look and feel how they want it to.
- Accessing values via koji-vcc should be a simple and straightforward process for anyone that wishes to remix your app. The base customizations are located on the top right of the project under the “Customization” section. (They may different depending on what project you remix)
- ![alt text](/docs/02_editor/vcc_reference/koji_customization.png)

## Available VCCs

### Image
  - Images can either be uploaded or obtained from a URL
  - ![alt text](/docs/02_editor/vcc_reference/upload_image.png)![alt text](/docs/02_editor/vcc_reference/rawurl_image.png)
  - If the image is loaded through the 'Upload' section, the image can be resized or the background can be removed and the image will be stored with the game.
  - If the image is loaded through the 'Advanced' section, it will use the image that is at this URL, so keep in mind that if the link dies the image will not appear.
  - Remember to keep images small as larger images will take a much longer time to load.
  - ![alt text](/docs/02_editor/vcc_reference/image_customization.png)

### Sound
  - Sounds can either be uploaded or obtained from a URL
  - ![alt text](/docs/02_editor/vcc_reference/upload_sound.png)![alt text](/docs/02_editor/vcc_reference/rawurl_sound.png)
  - If the sound is loaded through the 'Upload' section, the sound can be cropped to only play a portion of it and the sound will be stored with the game.
  - If the sound is loaded through the 'Advanced' section, it will use the sound that is at this URL, so keep in mind that if the link dies the sound will not play.
  - Remember to keep sounds to a small length, this is part of projects that usually take up the most space and cause the longest load times.
  - ![alt text](/docs/02_editor/vcc_reference/sound_customization.png)

### Color
  - Colors can be changed by clicking on the color or by pressing the 'Change' button and a color swatch will appear to choose a color.
  - ![alt text](/docs/02_editor/vcc_reference/color_customization.png)
  - ![alt text](/docs/02_editor/vcc_reference/color_choose.png)

### Text
  - Text can be changed by clicking the input field and typing the new text.
  - ![alt text](/docs/02_editor/vcc_reference/text_customization.png)

### Text Area
  - The difference between the 'Text Area' and just 'Text' is that a text area allows line breaks within it. Text can be changed by clicking in the input field and typing the new text.
  - ![alt text](/docs/02_editor/vcc_reference/textarea_customization.png)

### Boolean
  - Booleans are shown as toggle where it can off (gray) or on (blue)
  - ![alt text](/docs/02_editor/vcc_reference/booleanfalse_customization.png)![alt text](/docs/02_editor/vcc_reference/booleantrue_customization.png)

### Secret
  - Secret allows for keys to be set without the need to worry about them showing when a project gets remixed. It will only be visible in your project and no others even if they are remixed.
  - ![alt text](/docs/02_editor/vcc_reference/secret_customization.png)

### Range
  - Allows the user to drag a slider to specify a numeric value. Can be configured with a minimum value, maximum value, and a default increment/step size.

### Select
  - Lets the user choose from a predefined set of options

### 3D object
  - User can browse and import 3d OBJs from Google Poly

**Have ideas for VCC? Let us know!**

## Developer information

### Adding More Customization Areas
The files for the customization areas are located under Code/.koji/customization

![alt text](/docs/02_editor/vcc_reference/customization_folder_01.png)

Open the about_customization.md file to read more about creating customization options. Below is the text in the file.

![alt text](/docs/02_editor/vcc_reference/customization_md_text.png)

To add more customization options to the project, hover the mouse over the .koji/customization folder until you see the ‘Blue Plus’ in the image below.

![alt text](/docs/02_editor/vcc_reference/customization_folder_02.png)

A pop up will appear, shown below

![alt text](/docs/02_editor/vcc_reference/file_popup_01.png)

Enter the name of the new customization file and make sure that it is a .json file. It should be named in a way that refers to what the customization will do. Below is a file named ‘toggles.json’ as it will hold nothing but booleans or toggles for the user to change.

![alt text](/docs/02_editor/vcc_reference/file_popup_02.png)

Below is the basic setup for a customization file.

![alt text](/docs/02_editor/vcc_reference/basic_customization_file.png)

The “key” of the customization is the first text you will see, within that is where you will place all the values that will be accessible to the game code.
Below is the “@@editor” which is where you will add the ‘displays’ for the values you wish to show to the user.
  - Within the “@@editor” is the “key” which will reference all the values that will be placed in what is above.
  - The “name” is what will be displayed under the Customization section
  - The “icon” is the icon that will be displayed next to the name in the customization section. This is not needed but makes the project look nicer and the icon should represent what this customization does.
  - The “source” is the file that the values are being obtained from. They should generally point to the same file to keep the project clean and organized.
  - The “fields” is the area where you will add all the values you wish to be shown in the customization screen. You do not need to have all the values shown if they are not needed (best to have all options available to be customizable though).
  - The values that can be shown are listed above in the VCC References section, you can use image/sound/color/text/textarea/secret.
Below is an example of a boolean field and what it looks like in the customization screen.

![alt text](/docs/02_editor/vcc_reference/example_customization_file.png)

![alt text](/docs/02_editor/vcc_reference/example_customization_display.png)

![alt text](/docs/02_editor/vcc_reference/example_customization_menu.png)

Inside each field are the different values that are used to show that variable to the user.
  - The “key” is the key that will be used to access that variable inside the game code, i.e. using the variable above, “Koji.config.toggles.showLeaderboard” would return ‘true’.
  - The “name” is the name of the variable that is shown on the customization screen to the left of the variable. (The white “Show Leaderboard” text on the left)
  - The “description” should describe what this variable does in the game. Keep it short and to the point.
  - The “type” is what type of variable this is, image/sound/color/text/textarea/secret. Each will be displayed a different way, such as above, a boolean will be shown as a ‘toggle’ or ‘switch’ that can be set to true or false.

For use of the secret field, it would be best to check out the following project, https://withkoji.com/~sean/teams-sample it is a sample that shows how the secret field is used within a project.

### Composing VCCs

VCC types can be composed using higher-order controls. 

The array control lets you create a list of multiple values from a single VCC definition. To create an array, define the VCC type as `"type": "array<T>"` where `T` is a valid VCC type (e.g., `array<image>`). You can also use the shorthand `T[]`. You can optionally specify a maximum and/or minimum number of elements using the `arrayOptions` key. For example:
```
{
  "key": "myControl",
  "name": "My control",
  "type": "string[]",
  "arrayOptions": {
    "min": 2,
    "max": 10,
    "addItemLabel": "Add character" // if not set, the button shows "Add item"
  }
}
```

The object control lets you define an object or struct that is made up of more VCCs. For example, imagine if you were making a game that let users define multiple enemies. Each enemy might need a few pieces of metadata, like a name, image, strength level, and hitpoints. You can use the object type to compose a custom VCC that defines an enemy, and then optionally extend it as an array to create a list of enemies!

Example:
```
{
  "key": "enemies",
  "name": "Enemies",
  "description": "List of enemies in the game",
  "type": "object<Enemy>[]",
  "typeOptions": {
    "Enemy": {
      "name": {
        "name": "Enemy name",
        "description": "The name of the enemy",
        "type": "text"
      },
      "sprite": {
        "name": "Enemy sprite",
        "description": "Image to use for the enemy",
        "type": "image"
      },
      "strLevel": {
        "name": "Strength level",
        "type": "range",
        "typeOptions": {
          "min": 0,
          "max": 100,
          "step": 1
        }
      },
      "hitpoints": {
        "name": "Hitpoints",
        "type": "range",
        "typeOptions": {
          "min": 0,
          "max": 20,
          "step": 1
        }
      }
    }
  }
}
```

If you did not want the stuct to be available as an array, simply remove the brackets from the type signature (`"type": "object<Enemy>"`).

### VCC definition reference

#### Secret
```
{
  "key": "apiKey",
  "name": "Your Mailchimp API Key",
  "type": "secret",
  "typeOptions": {
    "suggestedKeystoreName": "mailchimp/api_key",
    "usageDescription": "Your Mailchimp API key is used to send emails collected by your app to Mailchimp",
    "procurementInstructions": "You can find your API key: https://mailchimp.com/help/about-api-keys/"
  }
}
```

Values of secret keys appear in VCC JSON as `koji-keystore://KEYSTORE_NAME` and are only resolved in the project's `env` as `process.env.KOJI_SECRETS_MAP`. You can access their unencrypted values by calling `Koji.resolveSecret(keyName)` in your app.

#### Range
```
{
  "key": "variableKey",
  "name": "Example variable",
  "description": "An example range variable",
  "type": "range",
  "typeOptions": {
    "min": 0,
    "max": 100,
    "step": 1
  }
}
```

#### Select
```
{
  "key": "variableKey",
  "name": "Example variable",
  "description": "An example select variable",
  "type": "select",
  "typeOptions": {
    "placeholder": "Choose an option...",
    "options": [
      { "value": "one", "label": "Value one" },
      { "value": "two", "label": "Value two" },
      { "value": "three", "label": "Value three" }
    ]
  }
}
```

#### 3D object
Definition:
```
{
  "key": "variableKey",
  "name": "Example variable",
  "description": "An example 3D object variable",
  "type": "obj"
}
```
Result:
```
{
  "obj": "https://objects.koji-cdn.com/c2755e73-1d5f-4b1e-9ede-51bba8e4b7ff/6dM1J6f6pm9/Mesh_Cat.obj",
  "mtl": "https://objects.koji-cdn.com/c2755e73-1d5f-4b1e-9ede-51bba8e4b7ff/6dM1J6f6pm9/Mesh_Cat.mtl",
  "texturePath": "https://objects.koji-cdn.com/c2755e73-1d5f-4b1e-9ede-51bba8e4b7ff/6dM1J6f6pm9/",
  "rawResources": [
    "https://objects.koji-cdn.com/c2755e73-1d5f-4b1e-9ede-51bba8e4b7ff/6dM1J6f6pm9/Mesh_Cat.mtl",
    "https://images.koji-cdn.com/c2755e73-1d5f-4b1e-9ede-51bba8e4b7ff/6dM1J6f6pm9/Tex_Cat.png",
    "https://objects.koji-cdn.com/c2755e73-1d5f-4b1e-9ede-51bba8e4b7ff/6dM1J6f6pm9/Mesh_Cat.obj"
  ],
  "thumbnailUrl": "https://images.koji-cdn.com/c2755e73-1d5f-4b1e-9ede-51bba8e4b7ff/6dM1J6f6pm9/thumbnail.jpg"
},
```
