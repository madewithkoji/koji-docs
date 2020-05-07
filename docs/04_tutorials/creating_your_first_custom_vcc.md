# Tutorial: Building Your First Custom VCC

## Why custom VCCs?

Custom VCCs will allow you to create new ways to interact with the people who will be remixing your games and apps.

Tile map editors, sound enhancers, level designers, and custom avatar creators are just a few of the custom VCCs that are being built, or are already currently in use!

The idea is that you, as a developer, can create a VCC that matches up closely with the type of game or app you are designing.

## Overview

**Goal**: The goal of this tutorial is to help you build and publish your first custom VCC. We will be recreating the `text` VCC that already exists in Koji, so by the end of the tutorial, you should be able to use your custom VCC interchangebaly with a `text` type VCC.

**Time**: You should be able to complete this tutorial in 30 minutes or less.

**Prerequisites**: Familiarity with the Koji editor and remix process is required. Familiarity with React and ES6 basics are a plus. 

## Tutorial

### Setting up the "consumer" app

In order to test and use our custom VCC, we will need a Koji app to act as the "consumer". To set that app up, we will remix the following scaffold:

https://withkoji.com/~seane/simple-react-scaffold

- Remix this project

- Once your editor has loaded, rename this project, to something like "Custom VCC Consumer"

If you look at the VCCs for this project, you will see one that says "Strings". Open that file, and you will see there is one option to change: "Page Title".

That value is currently controlled by a `text` VCC. Our goal will be to replace that VCC with one that we create ourselves!

### Setting up the "custom vcc app"

Custom VCCs are just Koji apps! You can create them in the same way that you create any app or game on Koji -- by remixing.

We are actually going to remix the same scaffold that we are using for our "Consumer", so create a new remix of the following scaffold:

https://withkoji.com/~seane/simple-react-scaffold

- Remix this project

- Once your editor has loaded, rename this project, to something like "My Custom Text VCC".

#### Adding the custom-vcc-sdk package

First we need to add the custom vcc sdk package.

- Open a new terminal using the "New Tab" at the bottom of the editor

- Navigate to the frontend folder `cd frontend`

- Install the package: `npm install --save @withkoji/custom-vcc-sdk`

- Close the terminal using `exit` and minimize the terminal panel by using the down caret `V` in the upper right of the terminal panel

#### Adding a text input and state management

We'll want an input to accept the value from the user, and we'll use a state to manage it in our React component.

- Open `/frontend/common/App.js` from the file browser

- Add a state via a constructor to the App class:

```
class App extends React.Component {
  constructor(props) {
    super(props);
    
    this.state = { value: '' };
  }
  ...
```

- Replace the contents of the `<Container>` component:

```
<Container>
  <input
    onChange={(e) => {
      this.setState({ value: e.currentTarget.value });
    }}
    value={this.state.value}
  />
</Container>
```

- Save the file, and you should now have a text input in your preview window

#### Import the VCC package & Initialize the custom VCC

First, we'll import the package we installed earlier. At the top of the `App.js` file, add the following:

```
import CustomVCC from '@withkoji/custom-vcc-sdk';
```

Now we will initialize the custom VCC and attach the instance to our app component. We'll also register the vcc, so that the "consumer" knows it's ready to use.

- In the `constructor` method, add the following:

```
this.customVCC = new CustomVCC();
```

- Add a `componentDidMount` method:

```
componentDidMount() {
  this.customVCC.register('300', '300');
}
```

### Testing the custom VCC

It would be very time consuming to build your custom VCC if you had to publish it each time you wanted to test it. Fortunately, we can easily test our VCC before it is published!

- While you are in your custom VCC project, click on the "Remote" tab, next to the "Live Preview".

- Click on the "Copy URL" to get the staging URL for your project.

- Open your "consumer" project

- In your consumer project, open the "Strings" customization file, and choose "Code" to view the raw json.

- For the `title` field, we will change the `type` to look like this, using the URL you just copied for `YOURURL`:

```
"type": "custom<YOURURL>"
```

- Save the file, and return to the "Visual" view of the customization file, and you should see your custom VCC!

### Connecting the custom VCC to the "consumer"

If you try typing into your custom VCC in your consumer app, you will see that nothing happens. That's because we need to connect the custom VCC to the consumer app.

We want our custom VCC app to be able to read and update the values in our json file in the "consumer" app. 

We will want to set our initial value to whatever is currently stored in the customization json file, and we will want to write a new value when that value is changed by the user.

Fortunately, there are several methods exposed by the `custom-sdk-vcc` package that will make this easy!

#### Getting the value from the "consumer"

We want our custom VCC to get the latest value from our json file. We can do this using the `onUpdate` method. 

- Switch back to your custom VCC app, and in `App.js`, add the following to your `componentDidMount` method, after the `register` call:

```
this.customVCC.onUpdate(({ value }) => {
  this.setState({ value });
});
```

Your `componentDidMount` should now look like this:

```
componentDidMount() {
  this.customVCC.register('300', '300');

  this.customVCC.onUpdate(({ value }) => {
    this.setState({ value });
  });
}
```

Whenever the value inside the "consumer" app changes, it will automatically update the state of our VCC component.

- Save this file, and return to your "consumer" app. Switch to the "Code" view, and then back to the "Visual" view to trigger a reload of your custom VCC. You should now see the correct value in the input!

#### Setting a new value using the custom VCC

If you try to change the value in the input box, you will see that the consumer app still doesn't update. This is because we are still not sending changes to the "consumer" app from our custom VCC.

To make this connection, we will use the `change` and `save` methods from the `custom-vcc-sdk` package.

- Update the `onChange` function for the input:

```
<input
  onChange={(e) => {
    this.customVCC.change(e.currentTarget.value);
    this.customVCC.save();
  }}
   value={this.state.value}
/>
```

This will update the value and trigger a save of the json file.

- Return to your "consumer" app. Switch to the "Code" view, and then back to the "Visual" view to trigger a reload of your custom VCC. You should now be able to update the title of the app using your own custom VCC!

### Publishing your custom VCC

You won't want to use the temporary staging URL, because that will change. Instead, you will want to publish your custom VCC so that you can use it easily in many projects (or share it with others!)

- On the left hand side of the editor, navigate to Advanced > Tools > Custom domains

- In the upper right hand of the editor, choose "Add domain"

- Under "Choose a Koji root domain", choose `koji-vccs.com`

- Inside of the domain input, give your vcc a unique name, like `diddys-custom-text-vcc`

- Choose "Add"

- In the upper left hand side of the editor, choose "Publish Now". Give your vcc a memorable name, and add a thumbnail if you would like!

- Click on "Publish App"

### Using a published custom VCC

Once your custom VCC has been published, you will want to use it! You can do that by replacing the `type` in your VCC with the domain input you chose in the last step:

Testing:

```
"type": "custom<YOURURL>"
```

Published:

```
"type": "custom<diddys-custom-text-vcc>"
```

Remember to replace the text with the domain input you chose!

If everything went well, you should now have created and published your first custom VCC!

### Extra Credit

Let's change the styling on our custom VCC to make it look more like a normal text VCC. If you have already published your custom VCC and updated the link in your "consumer" app, you will want to switch back to the staging URL, so that you can see your work in progress!

- Open up your App.js file in your custom VCC app. We can remove the unused styled components (`Icon`, `AppLogoSpin`, and `Content`).

First we'll handle some of the styling:

- Add the following styled components, near the top of the App.js file:

```
const InputContainer = styled.div`
    display: flex;
    flex-direction: column;
    width: 100%;

    input {
        width: 100%;
        background-color: rgb(255, 255, 255);
        color: rgb(17, 17, 17);
        border-width: 1px;
        border-style: solid;
        border-color: rgba(0, 0, 0, 0.1);
        border-image: initial;
        border-radius: 0px;
        padding: 8px;
        outline: none;
    }

    input:focus {
        outline: none;
        border-width: 1px;
        border-style: solid;
        border-color: rgb(21, 122, 251);
        border-image: initial;
    }

    .description {
        width: 100%;
        opacity: 0.4;
        font-size: 12px;
        line-height: 1;
        padding-top: 4px;
    }
`;

const Container = styled.div`
    background-color: #ffffff;
    color: #000000;
    padding: 16px;
    display: flex;
    align-items: start;
    width: calc(100% - 40px);
`;

const Label = styled.label`
    display: inline-flex;
    flex-direction: column;
    align-items: flex-end;
    padding-right: 16px;

    .name {
        font-size: 14px;
    }

    .variable-name {
        font-size: 10px;
        line-height: 1.5;
        color: rgb(102, 102, 102);
        opacity: 0.9;
        font-family: Menlo, Monaco, "Courier New", monospace;
    }
`;
```

We'll also use some additional information from the "consumer" application, to help provide some context to our custom VCC input.

- Update your state assignment to look like this:

```
this.state = {
	description: '',
	name: '',
	value: '',
	variableName: '',
};
```

- Update you `onUpdate` command to look like this:

```
this.customVCC.onUpdate(({ value, name, variableName, description }) => {
	this.setState({
  		description,
  		name,
  		value,
  		variableName,
	});
});
```

Now we will be pulling in more information about the field from the "consumer" app!

Last thing to do is to update the input. Replace the contents of the `<Container>` component with this:

```
<Label>
    <div class="name">{this.state.name}</div>
    <div class="variable-name">{this.state.variableName}</div>
</Label>
<InputContainer>
    <input
        onChange={(e) => {
            this.customVCC.change(e.currentTarget.value);
            this.customVCC.save();
        }}
        value={this.state.value}
    />
    <div class="description">{this.state.description}</div>
</InputContainer>
```

- Save your `App.js` file, return to your "consumer" app, reload the VCC, and you should see an updated VCC that looks just like Koji's `text` VCC!

Publish your changes to update your custom VCC's presentation!

### Wrapping up

We've created a replacement for an existing `text` VCC, and learned some of the basics about how a custom VCC "talks" to the consumer application.

If you are interested in creating more complex custom VCCs, you will probably want to check out the [package and documentation](https://github.com/madewithkoji/koji-custom-vcc-sdk).

You can also search for existing custom VCCs by searching for "vcc" on https://withkoji.com.

Reach out to @diddy on Discord if you have questions or comments about this tutorial =)
