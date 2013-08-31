tickertape
===========

A little engine for scroll-based storytelling.

Mapping out a basic API
------------------------

Some ideas about hos this thing will work. Tickertape will use the `tt` namespace.

Some ideas for syntax:

```js

tt('#selector').top(500, function(event){
	// do something when the top of the selected element reaches scroll position 500
});

// similarly:

tt('#selector').bottom(500, function(event){
	// do something when the bottom of the selected reaches scroll position 500
});

tt('#selector').middle(500, function(event){
	// you get the idea
});

// can also have it return a deferred object?
tt.on(500).then(function(){
	// general event when scroll position is reached
});

tt.config({
	selectorEngine: $,
	eventEngine: mediator,
	deferredEngine: $
});


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

// this fires on every scroll position update
// this will be the core of tt
tt.ticker(function(event){

});

```