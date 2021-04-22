### What For

1. We are facing H5 pages development.  
2. Have no idea with touch gestures.  
3. 300ms delay for clicks.  


## Here comes hammer!

Hammer helps you   add support for touch gestures to your page, and remove the 300ms delay from clicks.


## Supported gestures
- tap
- doubletap
- press
- pan   (direction horizontal only)
- swipe   (faster than a pan)



## Basic Usage
```html
<meta name="viewport" content="user-scalable=no, width=device-width,   
initial-scale=1, maximum-scale=1">
```

```javascript

var myHammer = new Hammer(myElement, myOptions);
myHammer.on('pan', function(ev){
    console.log(ev);
});
```

### with jQuery

```javascript
$(myselector).hammer().bind("tap", callback);
```


## add support for pinch and rotate

```javascript
myHammer.get('pinch').set({ enable: true });
myHammer.get('rotate').set({ enable: true });

myHammer.get('pan').set({ direction: Hammer.DIRECTION_ALL });
myHammer.get('swipe').set({ direction: Hammer.DIRECTION_VERTICAL });
```


###with jQuery
*we've met this problem in yijianbanjia*
```javascript
myElement.hammer({
    recognizers: [
        [Hammer.Pinch, {
            enable: true
        }],
        [Hammer.Swipe, {
            direction: Hammer.DIRECTION_HORIZONTAL
        }]
    ]
}).bind('pinch', function() {
    console.log('item pinched!');
});
```



## advanced usage
*Here we define our own recognizers with* **Manager** *class*  
*Which will give us more flexibility and accurateness*  
*When bind an element with several gestures*  

*key words : * **Manager recognizeWith requireFail**


```javascript
var mc = new Hammer.Manager(myElement, myOptions);

mc.add( new Hammer.Pan({ direction: Hammer.DIRECTION_ALL, threshold: 0 }) );
mc.add( new Hammer.Tap({ event: 'quadrupletap', taps: 4 }) );

mc.on("pan", handlePan);
mc.on("quadrupletap", handleTaps);

```


### recongizeWith && requireFail

```javascript
var myElement = document.getElementById('test-area');
var mc = new Hammer.Manager(myElement);

mc.add( new Hammer.Tap({ event: 'doubletap', taps: 2 }) );
mc.add( new Hammer.Tap({ event: 'singletap' }) );

mc.get('doubletap').recognizeWith('singletap');
mc.get('singletap').requireFailure('doubletap');

mc.on("doubletap singletap", function(ev) {
    console.log(ev.type +" ");
});
```



## source Code analysis

### Design philosophy
`Mobile Browser provides us with only three events:mousedown->mousemove->mouseup.`
1. addEventListener for element with three events.
2. mousedown->mouseup tap. different interval, times: doubletap, press.
3. mousedown->mouseover(->mouseup) different interval, pan, swipe.


### Source Code

* Hammer
    * Simple way to create a manager with a default set of recognizers.
    * Hammer.defaults
        * preset
        * cssProps

* Manager.
    * Main manager.
    * initialize Input,TouchAction,  


## Source Code

* InputHandler.
    * handle input events
    * Generate Input Object, send event to Recognizer.

* Input.
    * create new input type manager
    * handler : function()
    * TouchInput MouseInput TouchMouseInput SingleTouchInput PointerEventInput


### Source Code

* Recognizer.
    * recognizeWith
      * recognize simultaneous with an other recognizer.
    * requireFailure  
      * recognizer can only run when an other is failing

* event
    * type
    * scale
    * direction
    * deltaX deltaY



## Thanks All.