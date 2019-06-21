# iOS Issues

## Bottom bar cuts off bottom of app
- [viewport units don't report correct values](#viewport-units-dont-report-correct-values)

## Can't touch/click on item
- [no touch/click events for elements added after load.](#touch-events-on-added-DOM-nodes)
- [no touch events in iframe](#touch-events-in-iframe)

## Sounds not working
- [playing audio requires user gesture.](#Audio-workarounds)
- [ogg vorbis unsupported](#Ogg-Vorbis).

---

## touch events on added DOM nodes
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
## touch events in iframe 
iOS conditionally denies touch events. Choose either of these workarounds.
Taken from [here](https://stackoverflow.com/questions/41869122/touch-events-within-iframe-are-not-working-on-ios/50483933#50483933)


Add a dummy listener to the document within the iframe.
```js
  document.addEventListener('touchstart', {}); // in iframe
```
-OR-

Add a dummy listener to the top window
```js
  window.addEventListener('touchstart', {}); // in top window
```

---
## viewport units don't report correct values
100vh and window.innerHeight problems
Both iOS Safari and iOS Chrome do not report the correct viewport height. This bug is known and [currently unresolved](https://bugs.webkit.org/show_bug.cgi?id=141832) but may be revisited.

Checkout the various workarounds and chose one that fit your situation best.
- [lots of media queries](https://medium.com/@susiekim9/how-to-compensate-for-the-ios-viewport-unit-bug-46e78d54af0d)
- [eventbrite's solution](https://www.eventbrite.com/engineering/mobile-safari-why/)
- [bugfill package](https://github.com/rodneyrehm/viewport-units-buggyfill)

---
## Audio workarounds
iOS requires a user action to start playing audio. If you are using a framework like [P5](#P5-audio), iOS audio workaround are already baked in. Just make sure you are using the official loading methods and bind a user event to some method to get audio started.

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
With Vanilla JS there are 3 three general approaches. Which approach works best will depend on your setup.

1. Use an [immediate callback](#Callback) to play audio
2. Warm up a list of audio nodes [with a loop](#Loop)
3. Connect all sound sources to a warm [AudioContext](#AudioContext)


#### Callback
Play the sound in the callback of a user gesture.
```js
  var sound = new Audio();
  sound.src = "https://objects.koji-cdn.com/ae3b585d-6f50-47fc-91c2-fd574e826822/airhornclubsample1.mp3";

  // play an audio node on touchstart
  document.addEventListener('touchstart', () => {

    sound.play();
  });
```

#### Loop
Loop through a list of sounds to "warm" them up.

html
```html
  <button id="start"></button>
```

js
```js
  // a list of audio nodes
  var sounds = [...listOfAudioNodes];

  function warm(soundList) {
    // play and immediately pause each sound in the list

    soundList.forEach((sound) => {
      sound.play();
      sound.pause();
    })
  }

  // warm audio nodes with user gesture
  var startButton = document.getElementById('start');
  startButton.addEventListener('click', () => {

    warm(sounds);
  });

```

#### AudioContext
Play sounds through a warm [AudioContext](https://developer.mozilla.org/en-US/docs/Web/API/AudioContext)

Read more about the [Web Audio API](https://webaudioapi.com/book/Web_Audio_API_Boris_Smus_html/ch01.html#s01_1)

js
```js
// create an audio context
var audioCtx = new AudioContext();

// list of sounds as AudioBuffers
var audioBuffers = [...listOfAudioBuffers];

// warm audio context with user gesture
window.addEventListener('touchstart', function() {

	// create empty buffer
	var buffer = audioCtx.createBuffer(1, 1, 22050);
	var source = audioCtx.createBufferSource();
	source.buffer = buffer;

	// connect to output (your speakers)
	source.connect(audioCtx.destination);

	// play the file
	source.noteOn(0);

}, false);

// play sound
function playSound(audioBuffer) {
  var source = audioCtx.createBufferSource();
  source.buffer = audioBuffer;
  source.connect(audioCtx.destination);
  source.start(0);
}

// play a sound
playSound(audioBuffers[0]);
```

---
## Ogg Vorbis
Ogg Vorbis is [not supported](https://caniuse.com/#feat=ogg-vorbis) on iOS browsers.

---
## PWA Support
- https://medium.com/@myeris/getting-started-with-pwas-an-ios-nightmare-f0712c2f950