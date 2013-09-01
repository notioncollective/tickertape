tickertape
===========

A little engine for scroll-based storytelling.

**Note** that this is very much a work-in-progress, currently is a skeletal project with notes for implementation. 

What should it do?
-------------------
  * Provide a simple, dependable, and efficient engine for attaching events to scroll position.
  * Be as dependency-free as possible, but able to take advantage of other libraries when available.
  * Make it equally feasible to work primarily in HTML, JSON, or JavaScript -- or all of the above -- in orchestrating a scroll-based experience.
  * Have an extensible architecture that allows easy customization.
  * Have a straightforward and dorumented API.
  * Work really well on mobile platforms.

The core
---------

The core is the "ticker", the engine that manages monitoring the scroll behavior across clients as consistently as possible and firing callbacks based on specific scroll events.


Extensibility
-------------

### Transitions

These are simple re-usable transitions that are defined as a JavaScript callback function. They are always passed the tickertape event, as well as a non-predefined number of arguments. 

### Plugins

There should be a plugin archectecture that allows for bundling of transitions, data-attribute triggers, and miscellanious other JavaScript operations into one package. For instance:

* There could be a simple "css" plugin that performs css transitions based on scroll position.
* There could be a "media" plugin that fires specific media (i.e. video) events based on scroll position.
* There could be a "skrollr" plugin that mimics the behavior of the popular skrollr JavaScript library. 
* There could be a "myApp" plugin that handles all of application-specific logic for your project.

Scroll Events
--------------

The obvious core will be position based events, i.e. fired when `scrollTop` is at a specific number. But what other events do we want to have? How do you account for responsive design that has varying heights and positions based on the client type and size? This will take some thought.

Sketching out a basic API
------------------------

Some ideas about hos this thing will work. Tickertape will use the `tt` namespace.

Some ideas for syntax:


```js
// this fires on every scroll position update
// this will be the core of tt
tt(function(event){
	// code that runs on every "tick"
});

```

Or for a specific element:

```js

tt('#selector', function(event, element){
	// code that runs every "tick" on this particular element
});

```

What might be in the event object?

```js
{
	instance : tt, // instance of tt (object)
	scrollTop : 500, // scroll position at top of viewport (number),
	scrollSpeed: 50,
	scrollAccelleration: 1.5,
	scrollBottom : 800, // scroll position at bottom of viewport
	selectorEngine : jQuery, // uses jQuery's sizzle
	eventEngine : jQuery, // use jQuery for events
	deferredEngine : jQuery // use jQuery for deferred objects
	// ... what else?
}
```

If we specify an element:

```js

{
	// ... same as above, plus
	target,
	targetTop: 200,
	targetBottom: 400
}
```

Configuration allows for differend selector engines etc. Maybe by default it will look for jQuery?

```js
tt.start({
	selectorEngine: jQuery,
	eventEngine: new Mediator()
});

```

Events/callbacks for specific scroll positions:

```js
tt('#selector').on(500, function(event){
	// do something when the top of the selected element reaches scroll position 500
});

// or 

tt('#selector').top(500, function(event){
	// do something when the top of the selected element reaches scroll position 500
});

```

Similarly:

```js

tt(element).bottom(500, function(event){
	// do something when the bottom of the selected reaches scroll position 500
});

tt('#selector').middle(500, function(event){
	// you get the idea
});

```

Or fire an event/callback when a certain scroll position is reached:

```js
tt.on(500, function(event){
	// general event when scroll position is reached
});

```

Or do the same, but only the first time that position is reached

```js
tt.once(500, function(event){
	// general event when scroll position is reached
});
```


Or maybe set up a rule that only fires between a range of scroll positions?

```js
tt.between(500, 800, function(event){
	// fire some code
});

```


Register a "script" in object form

```js
tt.script({
	"#elementOne" : {
		"500" : function(event) {

		},
		"600" : function(event)
		}
	},
	".class" : {
		"top" : function(event) {

		},
		"200" : function(event) {

		}
	}
});

```

Maybe some sort of plugin syntax? This will need to be pretty well thought-out. 

```js
tt.plugin({
	name "tt-media",
	dataAttributes: {
		'media-start' : 'mediaStart',
		'media-stop' : 'mediaStop'
	},
	transitions: {
		'fadeAudio' : 'fadeAudioTransition'
	}
	init: function() {
		// the dataAttributes object automatically reads 
		// data-tt attributes and attaches them to the plugin object
		if(this.mediaStart) {

		}
	},
	fadeAudioTransition: function(tt, one, two) {
		// a transition definition
	}
});

```

```html
<div id="media-div" data-tt-media-start="500" data-tt-media-stop="600"></div>

```

Or at least a nice syntax for registering transitions?

```js
// it would be nice to have an easy hook to make reusable transitions
tt.registerTransition('testTransition', function(tt, one, two, three){
	tt.on(one, function(){
		// do some stuff
	});

	if(three) {
		tt.on(two, function(){
			// do something else
		});
	}
});

tt('selector').transition('testTransition', one, two, three);

tt('selector').transitions({
	'testTransition' : [100, 400, true],
	'anotherTransition' : [500, 600, 700]
});

```

```html
<div id="identifyer" data-tt-transitions="{testTransition: [one, two, three]}"></div>

```