# iOS issues

## FAQ
#### [Sounds doesn't work in iOS/Safari](#Audio-workarounds)
#### [Can't click on element/button in iOS/Safari](#click-event-workaround)
#### [The bottom bar cuts off the bottom or my app](#100vh-and-windowinnerHeight-problems)

---
## click event workaround
iOS won't register a click/touch event to an element added after DOM load. This example should get click events working in Safari and iOS for both static and updated elements. For more information about this quirk checkout https://www.quirksmode.org/blog/archives/2010/10/click_event_del_1.html

### JSBin 
https://jsbin.com/wabudabuqa/1/edit?html,css,js,output

### Example

html
```html
  <div id="button">Button</div>
```

js
```js
 
 // update button text function
 function setButton(button, message) {
    button.innerHTML = `<span id="buttoncontent">${message}</span>`;
 }
 
 // get button element
 var btn = document.getElementById("button");
 
 // update button text
 setButton(btn, "Start");
 
```

css
```css

  #button {
    cursor: pointer;
    user-select: none;
  }
  
  #button:hover {
    cursor: pointer;
  }
  
  #buttoncontent {
    pointer-events: none;
  }
  
```

---
## 100vh and window.innerHeight problems
Both iOS Safari and iOS Chrome do not report the correct viewport height. This bug is known and [currently unresolved](https://bugs.webkit.org/show_bug.cgi?id=141832) but may be revisited.

Checkout the various workarounds and chose one that fit your situation best.
- [lots of media queries](https://medium.com/@susiekim9/how-to-compensate-for-the-ios-viewport-unit-bug-46e78d54af0d)
- [eventbrite's solution](https://www.eventbrite.com/engineering/mobile-safari-why/)
- [bugfill package](https://github.com/rodneyrehm/viewport-units-buggyfill)

---
## Audio workarounds
iOS requires a user action to start playing audio. If you are using a framework like [P5](#P5-audio), iOS audio workaround are already baked in (based on warming up an [AudioContext](https://developer.mozilla.org/en-US/docs/Web/API/AudioContext)). Just make sure you are using the official loading methods and bind a user event to some method to get audio started.

### P5 audio
After loading audio with the [loadSound()](https://p5js.org/reference/#/p5.SoundFile/loadSound) method, to [startAudioContext](https://p5js.org/reference/#/p5.sound/getAudioContext) bind some code to start the audio context to a user gesture.

```js
  // bind some audio context warmup code to a touch event
  function touchStarted() {
    // check if audioContext is running
    if (getAudioContext().state !== 'running') {
      getAudioContext().resume();
    }

    // play a silent note
    var synth = new p5.MonoSynth();
    synth.amp(0);
    synth.play('A4', 0, 0, 0);
  }
```

### Vanilla JS audio
- immediate callback
- warming up audio with loop
- create an audio context and start it with a touch event
  - https://paulbakaus.com/tutorials/html5/web-audio-on-ios/
  - https://webaudioapi.com/book/Web_Audio_API_Boris_Smus_html/ch01.html#s01_1
- ogg vorbis

---
## notes
- audio won't play
    - immediate callback
    - warming up audio nodes with a loop
    - create an audio context and start it with a touch event
- pulldown refresh
- doesn't support pwa

--- 

https://medium.com/@myeris/getting-started-with-pwas-an-ios-nightmare-f0712c2f950

https://quirksmode.org/compatibility.html

## 
https://github.com/rgruesbeck/pong/commit/21b30adabf4240b4193d4131a66e81ef96359f2e#diff-7fe8b474a9819ab093e95b9e35513367L61

// p5 ios audio fix
https://github.com/processing/p5.js-sound/blob/117a69dc5345fa85c53a633be0b3dbb5b9253a86/src/sndcore.js#L158

http://frontend-9608daf0-76a5-41b1-98a1-f3fdd67576c1.koji-staging.com/

https://cdnjs.com/libraries/p5.js/