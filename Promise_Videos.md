# Video 1: SPA and AJAX
* A single-page application (SPA) is a web application or website that interacts with the user by dynamically rewriting the current page rather than loading entire new pages from the server
  * No reload or "refresh" within the user interface
    * JavaScript manipulates the DOM as the user interacts
  * User experience similar to a native application
* Data Serialzation-a way of turning live objects into flat objects that can be sent across the wire (this can be done with xml and json).
* If you have a JSON file and get an error when you try to parse it, it may be because what you have in the file is not valid JSON.
* Go to JSON.org to see the specification of JSON. Certain things that you can do in JavaScript (like having keys surrounded by single quotes) won't work in JSON.
* If you copy and paste JSON into JavaScript, it is valid JavaScript, and the same is true if you paste it into Python.
* When you run fetch, the result is a promise. Whenever we have a promise we need to use await. Await will wait for the promise to resolve and if the promise resolves, it will take the value it resolved to and return that value.
* You want to put await inside an asynchronous function.
* Body.JSON takes a response and parses it as a JSON object (gives us what's inside).
* await fetch("https://pokeapi.co/api/v2/pokemon.json()) retrieves the requested data
* In order to get the value that a paromise resolves to, you need to use a promise
* EX:
  * async()=>{
  * const response= await fetch("https://pokeapi.co/api/v2/pokemon")
  * const output= await response.json()
  * }
# Video 2: JavaScript Visualized-Promise Execution
* One way to create a promise is to use the new Promise Constructor
  * new Promise((resolve, reject)=>{
  * // async stuff here
  * })
* When the new Promise constructor is run, a Promise object is created, which contains the PromiseState, PromiseResult, PromiseFulfillReaction, PromiseFullfillReactions, PromiseHandled.
* We resolve the promise by calling "resolve". This sets the PromiseState to fulfilled and the PromiseResult to whatever value we passed to resolve
* EX: resolve("Done!"), PromiseResult is "Done!"
* We can reject a promise by calling "reject". This sets the PromiseState to rejected and the PromiseResult to whatever value we passed to reject.
* EX: reject("Fail!"), PromiseResult is "Fail!"
* PromiseFulfillReactions and PromiseRejectReactions contain something called Promise Reaction Records. We create a reaction record using .then
* Whenever we resolve a promise, resolve to added to the call stack, state is set to "fulfilled" and the reactions records handler receives the result.
* Usually, you want to initiate some kind of asynchronous task in the constructor (reading something from a file system, a timer, a network request). Once they return data we can use their callback function to either resolve with the data returned or reject if an error occurred.
* We can chain .thens to each other and have incremental promise results handling.
* EX:
  * new Promise((resolve, reject) =>{
    * resolve() 
  *  })
  *  .then(result => result * 2)
  *  .then(result => result * 2)
  *  .then(result => result * 2)
*  In a real application, you can incrementally handle promises and results, like by loading an image, then resizing it, adding a filter, and adding a watermark.
* EX:
  * function loadImage(src){
    * return new Promise((resolve, reject) => {
      * const img = new Image();
      * img.onload = () => resolve(img);
      * img.onerror = reject;
      * img.src = src;
    * })
  * }
  * loadImage(src)
  * .then(image => resizeImage(image))
  * .then(image => applyGrayscaleFilter(image))
  * .then(image => addWatermark(image))
* EX 2:
* new Promise((resolve))=>{
  * console.log(1)
  * resolve(2)
* }).then(result => console.log(result))
* 
* console.log(3)
* In the above function, the numbers are output as 1, 3, 2 because when .then creates the promise reaction record, the console logging of result (resolve(2)) is added to the microtask queue, so it is not immediately executed. console.log(3) is executed first, and nothing is left to execute so console.log(result) is removed from the mircotask queue and executed.

# Video 3: The Event Loop, Web APIs, (Micro)task Queue
* JavaScript allows us to use asyncronous tasks in a non-blocking way.
* JavaScript's call stack manages the execution of our program. JavaScript can only handle one task at a time, so if we have a task that requires some pretty heavy computation, it takes a while before JavaScript can continue with the rest of our program. We want to avoid long running tasks, but in real applications we often need them (user input, timers).
* Web APIs provide a set of interfaces that allow us to interact with a browser's features (fetch, setTimeOut, etc).
* Some Web APIs allow us to offload long running tasks to the browser.
* Web APIs that have asyncronous capabilities are either callback based or promise based
* EX: Geolocation API
  * navigator.geolocation.getCurrentPosition(
    * position => console.log(position)
    * error => console.error(error)
  * )
* Position is a successCallback (we were able to get the location)
* error is an errorCallback (something went wrong)
* In JavaScript, the event loop checks if the call stack is empty. If that's the case (nothing is running) it then gets the first available task from the task queue and moves it to the call stack and executes it.
* Whenever we work with promises, we're working with the microtask queue, a special queue dedicate to .then, .catch, .finally callbacks, function body execution after await, the queue microtask callback and the new MutationObserver callback.
* The event loop prioritizes the microtask queue over the task queue.
* When we invoke fetch, the fetch object is created, the .then handler creates a promise reaction record. The server still hasn't responded, so the console.log command at the end of the program is run. The promise reaction handler is pushed to the microtask queue so the result is logged.
* A microtask can also schedule another microtask, meaning the event loop could constantly work with the microtask and never get to the task queue.
* We can also promisify a callback based API. For example, we can wrap the getCurrent position with a new promise constructor, and pass resolve and reject for the success and error callbacks, as seen below.
* function getCurrentPosition(){
  * return new Promise((resolve, reject) => {
    * navigator.geolocation.getCurrentPosition(resolve, reject)
  * })
* }
* async function getUserLocation(){
  * try {
    * const location = await getCurrentPosition();
    * console.log(location)
  * } catch(error)
  * console.error(error)
  * }
* }
* Example:
  * Promise.resolve()
     * .then(() => console.log(1));
  *  setTimeOut(() => console.log(2), 10):
  *  queueMicrotask(() => {
    * console.log(3)
    * queueMicrotask(() => console.log(4))
  * });
  * console.log(5)
  * In the above code, the numbers output are 5, 1, 3, 4, 2 
