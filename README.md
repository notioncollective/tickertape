tickertape
===========

A little engine for scroll-based storytelling.

**Note** that this is very much a work-in-progress, currently is a skeletal project with notes for implementation. 

Mapping out a basic API
------------------------

Some ideas about hos this thing will work. Tickertape will use the `tt` namespace.

Some ideas for syntax:


```js
// this fires on every scroll position update
// this will be the core of tt
tt.ticker(function(event){
	// code that runs on every "tick"
});

```

What might be in the event object?

```js
{
	instance : tt, // instance of tt (object)
	scrollTop : 500, // scroll position at top of viewport (number)
	scrollBottom : 800, // scroll position at bottom of viewport
	selectorEngine : jQuery, // uses jQuery's sizzle
	eventEngine : jQuery, // use jQuery for events
	deferredEngine : jQuery // use jQuery for deferred objects
	// ... what else?
}
```

Configuration allows for differend selector engines etc. Maybe by default it will look for jQuery?

```js
tt.config({
	selectorEngine: $,
	eventEngine: $,
	deferredEngine: $
});

```


Events/callbacks for specific scroll positions:

```js
tt('#selector').top(500, function(event){
	// do something when the top of the selected element reaches scroll position 500
});

```

Similarly:

```js

tt('#selector').bottom(500, function(event){
	// do something when the bottom of the selected reaches scroll position 500
});

tt('#selector').middle(500, function(event){
	// you get the idea
});

```

Or fire an event/callback when a certain scroll position is reached:

```js
// can also have it return a deferred object?
tt.on(500).then(function(){
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
tt.register({
	"selector" : {
		"500" : {
			// some settings here?
		},
		"600" : {
			// more settings here?
		}
	},
	"selector" : {
		"top" : function() {

		},
		"200" : function() {

		}
	}
});

```

Maybe a way to register css transitions?

```js
tt('selector').transition(
	[100, 400, 800],
	{
		// css rules for 100
	},
	{
		// css rules for 400
	},
	function(event) {

		// execute js

		return {
			// return css rules for 400?
		}
	}
);

```

Maybe some sort of plugin syntax?

```js
tt.plugin({
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
	})
});

```

```html
<div id="identifyer" data-tt-transitions="{testTransition: [one, two, three]}"></div>

```