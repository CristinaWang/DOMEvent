# DOM Event

## Background
> Click, touch, load, drag, change, input, error, resize — [the list of possible DOM events](https://developer.mozilla.org/en-US/docs/Web/API/Event) is lengthy. Events can be triggered on any part of a document, whether by a user’s interaction or by the browser. They don’t just start and end in one place; they flow through the document, on a life cycle of their own. **This life cycle** is what makes DOM events so extensible and useful. As a developer, you should understand how DOM events work, so that you can harness their potential and build engaging experiences.

## How to add a DOM event listeners or Remove it

* **Javascipt: **
  * element.addEventListener(eventName, handler, useCapture) / element.removeEventListener(eventName, handler, useCapture)
    >**handler**: Need to have a reference to the handler if you are going to remove it later
     **useCapture**: Whether the handler should be fire during the capture phrase
  * myEl.onclick = handler; _(Can be override since it's an attribute)_;
  
* **jQuery:**
  * $.on(event, handler) & $.one(event, handler) / $.off(event, handler)
  * $.click(handler)
  * $.bind()/$.delegate()/$.live() / $.unbind(event, handler) _(Superseded since jQuery 1.7)_
  

### $.on(event, handler)

$element.on(event, handler);
$element.on(event, selector, handler);

### $.click(handler) and $.on('click', handler)
$.click(handler) is same as $.on('click', handler), and $.click() is same as $.trigger('click'). And the difference is:
* Delagrate ($.on()offers additional flexibility in allowing you to delegate events fired by children)
```javascript
$('ul').on(event, 'li', function(){});
```
* namesapce
```javascript
$("#element").on("click.someNamespace", function() { console.log("anonymous!"); });
$("#element").off("click.someNamespace");
```
* Flexibility 
```javascript
$( "div.test" ).on({
  click: function() {
    $( this ).toggleClass( "active" );
  }, mouseenter: function() {
    $( this ).addClass( "inside" );
  }, mouseleave: function() {
    $( this ).removeClass( "inside" );
  }
});
```

#### How jQuery add evenet listener:
```javascript
// Bind the global event handler to the element
if ( elem.addEventListener ) {
   elem.addEventListener( type, eventHandle, false );
} else if ( elem.attachEvent ) {
	elem.attachEvent( "on" + type, eventHandle );
}
```
Further reading of [source code](https://github.com/jquery/jquery/blob/6e995583a11b63bf1d94142da6408955ee93e7cc/src/event.js#L97-102)

## Event phrase
***
![Alt text](/eventflow.png)


###### [Demo: Slow motion event path](http://jsbin.com/exezex/4/edit?css,js,output)

### Capture phrase
The job of the capture phase is to build the propagation path, which the event will travel back through in the bubbling phase.
Prevent any clicks from firing in a certain element if the event is handled in the capture phase.
```javascript
var form = document.querySelector('form');

form.addEventListener('click', function(event) {
  event.stopPropagation();
}, true);
```

### Target phrase
In the case of nested elements, mouse and pointer events are always targeted at the most deeply nested element. 

### Bubble phrase

A [demo](http://jsbin.com/unuhec/4/edit?html,css,js,output) to identify event phases. 

## The Event Object
The properties of event object:
* **type (string)**: This is the name of the event.
* **target (node)**
  This is the DOM node where the event originated. _(The core of Onion)_
* **currentTarget (node)**
  This is the DOM node that the event callback is currently firing on.
* **bubbles (boolean)**
  This indicates whether this is a “bubbling” event (which we’ll explain later).
* **preventDefault (function)**
* **stopPropagation (function)** _Interrupting the path of the event at any point on its journey_
	```javascript
	child.addEventListener('click', function(event) {
 		event.stopPropagation();
	});

	parent.addEventListener('click', function(event) {
	 // If the child element is clicked
	 // this callback will not fire
	});
	```
* **stopImmediatePropagation (function)** 
_This prevents any callbacks from being fired on any nodes further along the event chain, including any additional callbacks of the same event name on the current node._
```javascript
child.addEventListener('click', function(event) {
	event.stopImmediatePropagation();
});
child.addEventListener('click', function(event) {		 
	// If the child element is clicked
	// this callback will not fire
});
```
* **cancelable (boolean)**
  This indicates whether the default behaviour of this event can be prevented by calling the event.preventDefault method.
* **defaultPrevented (boolean)**
  This states whether the preventDefault method has been called on the event object.
* **eventPhase (number)**
  The phase that the event is currently in: none (0), capture (1), target (2) or bubbling (3).

[Demo](https://jsfiddle.net/xcLLp7ev/) to view event object.

## Custom DOM Events
***
```javascript
var myEvent = new CustomEvent("myevent", {
  detail: {
    name: "Wilson"
  },
  bubbles: true,
  cancelable: false
});

// Listen for 'myevent' on an element
myElement.addEventListener('myevent', function(event) {
  alert('Hello ' + event.detail.name);
});

// Trigger the 'myevent'
myElement.dispatchEvent(myEvent);
```
[Demo](http://jsbin.com/emuhef/1/edit?html,css,js,output)

## Delegate Event Listeners
***
* javacript
  ```javascript
  var list = document.querySelector('ul');

  list.addEventListener('click', function(event) {
    var target = event.target;

    while (target.tagName !== 'LI') {
      target = target.parentNode;
      if (target === list) return;
    }

    // Do stuff here
  });
  ```
* Jquery
  ```javascript
  $('ul').on('click', 'li', function(){});
  ```
Or use FT Lab’s [ftdomdelegate](https://github.com/ftlabs/ftdomdelegate)
  

## Useful Events
***
Debounced callback to normalize the callback rate and prevent extreme thrashing in the layout.
* ONBEFOREUNLOAD [Demo](http://jsbin.com/inelaj/2/edit)
  * Not works in react/angular single page application
  * Note that assigning an onbeforeunload handler prevents the browser from caching the page, thus making return visits a lot slower. Also, onbeforeunload handlers must be synchronous.
* ERROR

  ```javascript
  imageNode.addEventListener('error', function(event) {
      image.style.display = 'none';
  });
  ```
  
>##### Tip :sparkles: Use debounce when doing resize/scroll etc event

## Reference
* [An Introduction To DOM Events](https://www.smashingmagazine.com/2013/11/an-introduction-to-dom-events/)
* [UI Event](https://www.w3.org/TR/DOM-Level-3-Events/#dom-event-architecture)
* [Event compatibility tables](https://www.quirksmode.org/dom/events/)







