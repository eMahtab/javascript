# Event propagation : bubbling and capturing
When an event happens – the most nested element where it happens gets labeled as the “target element” (event.target).

1. Then the event moves down from the document root to event.target, calling handlers assigned with addEventListener(..., true) on the way (true is a shorthand for {capture: true}).

2. Then handlers are called on the target element itself.

3. Then the event bubbles up from event.target to the root, calling handlers assigned using on<event>, HTML attributes and addEventListener without the 3rd argument or with the 3rd argument false/{capture:false}.
```  
Each handler can access event object properties:

event.target – the deepest element that originated the event.
event.currentTarget (=this) – the current element that handles the event (the one that has the handler on it)
event.eventPhase – the current phase (capturing=1, target=2, bubbling=3).
Any event handler can stop the event by calling event.stopPropagation(), but that’s not recommended, because we can’t really be sure we won’t need it above, maybe for completely different things.
```
  
The capturing phase is used very rarely, usually we handle events on bubbling. And there’s a logic behind that.

In real world, when an accident happens, local authorities react first. They know best the area where it happened. Then higher-level authorities if needed.

The same for event handlers. The code that set the handler on a particular element knows maximum details about the element and what it does. A handler on a particular <td> may be suited for that exactly <td>, it knows everything about it, so it should get the chance first. Then its immediate parent also knows about the context, but a little bit less, and so on till the very top element that handles general concepts and runs the last one.

# Event Delegation :

The idea is that if we have a lot of elements handled in a similar way, then instead of assigning a handler to each of them – we put a single handler on their common ancestor.

In the handler we get event.target to see where the event actually happened and handle it.

## Javascript Event Loop

1. What is Javascript Event loop https://www.youtube.com/watch?v=EI7sN1dDwcY (Java Brains)

2. Strategies for Event loop https://www.youtube.com/watch?v=IvLltoCt8QU (Java Brains)
  
## The main thread is overworked & underpaid (Chrome Dev Summit 2019) - Surma
   https://www.youtube.com/watch?v=7Rrv9qFMWNM
  
  

## Web Worker

```html
<!DOCTYPE html>
<html>
<body>

<p>Count numbers: <output id="result"></output></p>
<button onclick="startWorker()">Start Worker</button>
<button onclick="stopWorker()">Stop Worker</button>

<script>
var w;

function startWorker() {
  if (typeof(Worker) !== "undefined") {
    if (typeof(w) == "undefined") {
      w = new Worker("demo_worker.js");
    }
    w.onmessage = function(event) {
      document.getElementById("result").innerHTML = event.data;
    };
  } else {
    document.getElementById("result").innerHTML = "Sorry! No Web Worker support.";
  }
}

function stopWorker() {
  w.terminate();
  w = undefined;
}
</script>

</body>
</html>  
```  
#### demo_worker.js
```javascript
var i = 0;

function timedCount() {
  i = i + 1;
  postMessage(i);
  setTimeout("timedCount()",500);
}

timedCount();  
```  
![Web Worker Example](web-worker.JPG?raw=true)  
  
## Service Worker
https://www.youtube.com/watch?v=JYXXGNFJjwc  Service Workers - The State of the Web (Google Chrome Developers)
  
## Progressive Web App (PWA) :
  
Progressive Web Apps (PWA) are built and enhanced with modern APIs to deliver enhanced capabilities, reliability, and installability while reaching anyone, anywhere, on any device with a single codebase.
  
https://web.dev/what-are-pwas/
  
  

  
