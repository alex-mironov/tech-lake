## JavaScript
### Other lists
- [JavaScript Questions](https://github.com/lydiahallie/javascript-questions) - A long list of (advanced) JavaScript questions, and their explanations
- [JavaScript Interview Questions & Answers](https://github.com/sudheerj/javascript-interview-questions)
- [Awesome Interviews](https://github.com/DopplerHQ/awesome-interview-questions) - Front-end related questions
- [Frontend Interview Questions](https://dev.to/m_midas/52-frontend-interview-questions-javascript-59h6) - 52 Frontend Interview Questions - JavaScript

### Questions
- What types exist in JavaScript (Symbol)
- Arrow functions
- call/apply/bind
- Difference var vs let/const
- What is a potential pitfall with using `typeof bar === "object"`
- `{} === {}`
- What is a “closure” in JavaScript? Provide an example
- ****What are iterators and iterables in JavaScript? Any built-in iterators that you know?****
- What is the difference between the function declarations below?
- What is the DOM?
- What is the event loop?
- How does prototypal inheritance work, and how is it different from classical inheritance? (this is not a useful question IMO, but a lot of people like to ask it)
- How does `this` work?
- What is event bubbling and how does it work?
- What’s the difference between `async` and `defer`?
- What is the difference?

```javascript
promise.then(() => {
    // …
}, () => {
    // …
})

promise.then(() => {
    // …
}).catch(() => {
    // …
})
```

- What does it do?

```javascript
(function(){
  var a = b = 3;
})()
```

- What is the output?

```javascript
(function() {
    console.log(1); 
    setTimeout(function(){console.log(2)}, 1000); 
    setTimeout(function(){console.log(3)}, 0); 
    Promise.resolve().then(() => console.log(4));
    new Promise((resolve, reject) => resolve()).then(() => console.log(5))
    console.log(6);
})();
```

- And here?

```javascript
var myObject = {
    foo: "bar",
    func: function() {
        var self = this;
        console.log("outer func:  this.foo = " + this.foo);
        console.log("outer func:  self.foo = " + self.foo);
        (function() {
            console.log("inner func:  this.foo = " + this.foo);
            console.log("inner func:  self.foo = " + self.foo);
        }());
    }
};
myObject.func();
```

- And here?

```javascript
console.log(1 +  "2" + "2");
console.log(1 +  -"1" + "2");
console.log( "A" - "B" + "2");
```

- What will be the result?

```javascript
console.log(false == '0')
```

## React
### Other lists
- [The Best Frontend JavaScript Interview Questions (written by a Frontend Engineer)](https://performancejs.com/post/hde6d32/The-Best-Frontend-JavaScript-Interview-Questions-%28written-by-a-Frontend-Engineer%29)
- [React Interview Questions & Answers](https://github.com/sudheerj/reactjs-interview-questions)
### Questions
- Is it safe to call react hooks based on a condition?
- What are the rules/limitation that apply to hooks? - loops, conditions, or nested functions
- In which lifecycle event / hook do you make AJAX requests and why?
- Difference between Component and PureComponent
- What is a context?
- What happens when you call setState?
- What’s the difference between an **Element** and a **Component** in React?
- What are **refs** in React and why are they important?
- What are **keys** in React and why are they important?
- What does shouldComponentUpdate do and why is it important?
- In what case would you use React.Children.map(props.children, () => )
- What is the second argument that can optionally be passed to setState and what is its purpose?
- Why setState is async?
- What is code splitting? How do you achieve that in react?
- How to unsubscribe from events when using hooks?
- What are portals in React?
- Describe 3 ways to pass information from a component to its **parent**.
	- **Callback functions**
	- **Context**
	- **Ref**

## CSS
- Tell me about the box model
- What CSS selectors do you use?
	- [https://jsfiddle.net/alex_myronov/3m74xor8/1/](https://jsfiddle.net/alex_myronov/3m74xor8/1/
	- `div + a`
	- `X > Y`
	- `X:nth-child(n)`
- What is margin collapsing?
- What responsive design techniques do you use?
- What will happen if you set `margin: -10px;`?

## System Design
### Questions good to hear from candidate

[System Design Interview Tips - DEV Community](https://dev.to/thawkin3/system-design-interview-tips-2ohe)
- **Which features are most important?** (For example, designing Twitter is no small feat, so the interviewer may tell you to focus on just the news feed.)
- **How many daily active users are there?** (It’s important to know if this a v1 product for a tiny startup or an established product that has millions of users.)
- **Can I leverage existing cloud providers like AWS, GCP, or Azure?** (Sometimes they want you to build something from scratch, but most of the time it’s perfectly acceptable to say that you’ll use existing infrastructure like a load balancer (ELB), auto-scaling groups, document storage (S3), message queues (SQS), cache (Redis), or CDN (CloudFront).
- **What types of data are we dealing with?** (It’s good to know if you’re just working with text or if you’re storing images or videos as part of your service. The data type and data structure will likely influence your choice of database.)
- **Do we need to consider [X]?** (The beginning of the interview is all about establishing scope, so ask your interviewer about various concerns that should be considered in scope or out of scope. Oftentimes they’ll let you stick with the simpler approach to begin with and then tackle the harder problems toward the end of the interview if there’s time.)
- **Can I assume that [X]?** (Make your assumptions known to your interviewer. This will ensure that you’re both on the same page. The interviewer may also provide additional clarification for you here based on your previous assumptions.)

### Questions
- What is the difference between **throughput** and **latency**?
- Tell me about JWT. What are the benefits and drawbacks? How would you invalidate compromised tokens?
- What are indexes and why are they needed? What are the drawbacks of indexes?
- When you upload the file what is the type of request?
- What is DNS?
- What is a CDN?
- How would you approach caching (and cache invalidation)?
- What is REST, and why do people use it?
- What is the difference between `HEAD` and `OPTIONS` requests?
- What is the difference between `PUT` and `PATCH` requests?
- I type URL into the browser’s address bar and hit _Enter_, what happens under the hood till the time when WEB page is fully loaded?
- How HTTP works? Structure of request. Structure of response. What are cookies?
- How does page load? In what order do assets load and process?
- Web-page rendering. Critical rendering path, CSSOM, DOM
- What is CORS?
- Describe a few ways to communicate between a server and a client. Describe how a few network protocols work at a high level (IP, TCP, HTTP/S/2, UDP, RTC, DNS, etc.)
- What is HTTP `Keep-Alive` and how do they work?

### Tasks
#### Design Autocomplete
Talk me through a full stack implementation of an autocomplete widget. A user can type text into it, and get back results from a server.
* How would you design a frontend to support the following features:
	* Fetch data from a backend API
	* Render results as a tree (items can have parents/children - it’s not just a flat list)
	* Support for checkbox, radio button, icon, and regular list items - items come in many forms
* What does the component’s API look like?
* What does the backend API look like?
* What perf considerations are there for complete-as-you-type behavior? Are there any edge cases (for example, if the user types fast and the network is slow)?
* How would you design the network stack and backend in support of fast performance: how do your client/server communicate? How is your data stored on the backend? How do these approaches scale to lots of data and lots of clients?


## [Node.js](https://github.com/learning-zone/nodejs-basics#q-why-should-you-separate-express-app-and-server)

### Other lists
- [Node.js](https://github.com/learning-zone/nodejs-basics#q-why-should-you-separate-express-app-and-server)
- [Back-End Developer Interview Questions](https://github.com/arialdomartini/Back-End-Developer-Interview-Questions) - A list of back-end related questions you can be inspired from

### Questions
#### 1. What is the preferred method of resolving unhandled exceptions in Node.js?

What will happen if we throw an unhandled exception?
	* What if exception will be thrown in Promise?
	* What if Promise will be rejected?

Unhandled exceptions in Node.js can be caught at the `Process` level by attaching a handler for `uncaughtException` event.

```javascript
process.on('uncaughtException', (err) => {
  console.log(`Caught exception: ${err}`);
});
```

However, `uncaughtException` is a very crude mechanism for exception handling and may be removed from Node.js in the future. An exception that has bubbled all the way up to the `Process` level means that your application, and Node.js may be in an undefined state, and the only sensible approach would be to restart everything.

The preferred way is to add another layer between your application and the Node.js process which is called the [domain](https://nodejs.org/api/domain.html).

Domains provide a way to handle multiple different I/O operations as a single group. So, by having your application, or part of it, running in a separate domain, you can safely handle exceptions at the domain level, before they reach the `Process` level.

Few events are :
1. exit
2. disconnect
3. unhandledException
4. rejectionHandled

#### 2. What are closure, object prototype, event loop?

#### 3. What are clusters and worker threads, and when would you use them?

Worker threads are for when you want to run long-running synchronous tasks, and you don't want to block the event loop. It runs the code in a separate thread, and can communicate the results back to the main thread.

Cluster workers are separate instances of your Node process, which are primarily used for load balancing incoming requests across multiple processes, to take advantage of multi-core processors.
###### How does nodeJS event loop work
- Talk about `setTimeout` / `setInterval` / `setImmediate` / `process.nextTick`

#### 4. [What are the middleware functions in Node.js?](https://github.com/learning-zone/nodejs-basics#q-what-are-the-middleware-functions-in-nodejs)
Middleware functions are functions that have access to the **request object (req)**, the **response object (res)**, and the `next` function in the application's request-response cycle.

Middleware functions can perform the following tasks:

- Execute any code.
- Make changes to the request and the response objects.
- End the request-response cycle.
- Call the next middleware in the stack.
###### [What are some of the most popular packages of Node.js?](https://github.com/learning-zone/nodejs-basics#q-what-are-some-of-the-most-popular-packages-of-nodejs)
###### [How can you make sure your dependencies are safe?](https://github.com/learning-zone/nodejs-basics#q-how-can-you-make-sure-your-dependencies-are-safe)

###### [What are the types of memory leaks in node.js?](https://github.com/learning-zone/nodejs-basics#q-what-are-the-types-of-memory-leaks-in-nodejs)

The common causes of Memory Leaks in Node.JS are:

**1. Global variables:**

This is one of the most common causes of leaks in Node. Due to the nature of JavaScript as a language, it is very easy to add to global variables and resources. If these are not cleaned over time, they keep adding up and eventually crash the application.

**2. Closures:**

Closures memorize their surrounding context. When a closure holds a reference to a large object in heap, it keeps the object in memory as long as the closure is in use.

This implies easily ending up in situations where a closure holding such a reference can be improperly used leading to a memory leak.

**3. Timers & Events:**

The use of setTimeout, setInterval, Observers, and event listeners can cause memory leaks when heavy object references are kept in their callbacks without proper handling.

**4. Multiple references:**

If you reference the same object from multiple objects, it can lead to a memory leak if one of the references is garbage collected while the other one is left dangling.
###### [How to gracefully shutdown Node.js Server?](https://github.com/learning-zone/nodejs-basics#q-how-to-gracefully-shutdown-nodejs-server)
The graceful shutdown of our application indicates when all of the resources it used and all of the traffic and/or data processing what it handled are closed and released properly.

Possible scenarios for a graceful web server shutdown:

- Handle process kill signal
- Stop new requests from client
- Close all data process
- Exit from process



#### 5. What are the streams in Node.js?
Would be nice to hear about Writable, Readable, Duplex, Transform streams
#### 6. What is middleware?
