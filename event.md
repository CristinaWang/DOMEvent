# DOM Event

## Background
Click, touch, load, drag, change, input, error, resize — [the list of possible DOM events](https://developer.mozilla.org/en-US/docs/Web/API/Event) is lengthy. Events can be triggered on any part of a document, whether by a user’s interaction or by the browser. They don’t just start and end in one place; they flow through the document, on a life cycle of their own. This life cycle is what makes DOM events so extensible and useful. As a developer, you should understand how DOM events work, so that you can harness their potential and build engaging experiences.

## How to add a DOM event listeners or Remove it
* Javascipt way: element.addEventListener(eventName, handler, useCapture) and element.removeEventListener(eventName, handler)
  * handler: Need to have a reference to the handler if you are going to remove it later (While jQuery:)
    ```javascript
    $("#element").on("click.someNamespace", handler);
    $("#element").off("click.someNamespace");
    ```
  * useCapture: Wether the handler should be fire during the capture phrase
* jQuery way:
  * $.on('click', handler) and $.off('click', handler)
  * $.click(handler)
  * $.bind('click', handler) and $.unbind('click', handler)
  
Explain the difference in [here](https://github.com/CristinaWang/DOMEvent/blob/master/Multiple-ways-to-listen-event.md).

## Event phrase
(/eventflow.png)
[Demo: Slow motion event path](http://jsbin.com/exezex/4/edit?css,js,output)

### Capture phrase
Used for prevent some behavior in bubbling phrase:
```javascript
var form = document.querySelector('form');

form.addEventListener('click', function(event) {
  event.stopPropagation();
}, true);
```

### Target phrase
An event reaching the target is known as the target phase.
If you have listened for a click event on a <div> element, and the user actually clicks on a <p> element in the div, then the <p> element will become the event target. The fact that events “bubble” means that you are able to listen for clicks on the <div> (or any other ancestor node) and still receive a callback once the event passes through.

### Bubble phrase


A [demo](http://jsbin.com/unuhec/4/edit?html,css,js,output) to identify event phases. 

## The Event Object
The properties of event object:
* type (string): This is the name of the event.
* target (node)
  This is the DOM node where the event originated.
* currentTarget (node)
  This is the DOM node that the event callback is currently firing on.
* bubbles (boolean)
  This indicates whether this is a “bubbling” event (which we’ll explain later).
* preventDefault (function)
* stopPropagation (function)
* stopImmediatePropagation (function) //todo example
  This prevents any callbacks from being fired on any nodes further along the event chain, including any additional callbacks   of the same event name on the current node.
* cancelable (boolean)
  This indicates whether the default behaviour of this event can be prevented by calling the event.preventDefault method.
* defaultPrevented (boolean)
  This states whether the preventDefault method has been called on the event object.
* eventPhase (number) //todo example
  The phase that the event is currently in: none (0), capture (1), target (2) or bubbling (3).
  
## Custom DOM Events
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
Debounced callback to normalize the callback rate and prevent extreme thrashing in the layout.
* ONBEFOREUNLOAD [Demo](http://jsbin.com/inelaj/2/edit)
* ERROR
  ```javascript
  imageNode.addEventListener('error', function(event) {
      image.style.display = 'none';
  });
  ```



# More :sparkles: 
* What the difference between dom.addEventListener('click', handler) vs $.on('click', handler) vs $.click(handler) vs $.bind('click', handler)

## Reference
* [An Introduction To DOM Events](https://www.smashingmagazine.com/2013/11/an-introduction-to-dom-events/)
* [UI Event](https://www.w3.org/TR/DOM-Level-3-Events/#dom-event-architecture)
* [Event compatibility tables](https://www.quirksmode.org/dom/events/)







