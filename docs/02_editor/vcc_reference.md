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
