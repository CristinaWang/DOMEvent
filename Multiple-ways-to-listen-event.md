# Listening For DOM Events

## Javascipt
element.addEventListener(eventName, handler, useCapture) / element.removeEventListener(eventName, handler);
myEl.onclick = function(event){alert('Hello world');}; (Can be override since it's an attribute);


## jQuery
* $.on('click', handler) / $.off('click', handler)
* $.click(handler)
* $.bind('click', handler) / $.unbind('click', handler) (Superseded since jQuery 1.7)

How jQuery add evenet listener:
```javascript
// Bind the global event handler to the element
if ( elem.addEventListener ) {
   elem.addEventListener( type, eventHandle, false );
} else if ( elem.attachEvent ) {
	elem.attachEvent( "on" + type, eventHandle );
}
```
Further reading of [source code](https://github.com/jquery/jquery/blob/6e995583a11b63bf1d94142da6408955ee93e7cc/src/event.js#L97-102)

### $.click(handler) and $.on('click', handler)
$.click(handler) is same as $.on('click', handler), and $.click() is same as $.trigger('click'). And the difference is:
* Delagrate ($.on()offers additional flexibility in allowing you to delegate events fired by children)
```javascript
$('ul').on('click', 'li', function(){});
```
* namesapce
```javascript
$("#element").on("click.someNamespace", function() { console.log("anonymous!"); });
$("#element").off("click.someNamespace");
```
* Flex 
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
