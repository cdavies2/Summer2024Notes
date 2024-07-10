# Promise
* The Promise object represents the completion or failure of an asynchronous operation.
* It is a returned object to which you attach callbacks, instead of passing callbacks into a function.
* A Promise is a proxy for a value not necessarily known when the promise is created. It lets asynchornous methods return values like synchronous methods: instead of immediately returning the final value, the asynchronous method returns a promise to supply the value at some point in the future.
* A Promise is in one of these states....
    * Pending-initial state, neither fulfilled nor rejected
    * Fulfilled-meaning that the operation was completed successfully
    * Rejected-meaning that the operation failed.
*  A pending promise can be fulfilled with a value or rejected with a reason (error). If the promise has already been fulfilled or rejected when a corresponding handler is attached, the handler will be called, so there's no race condition between an asynchronous operation completing and its handlers being attached.
*  A promise is settled if it is either fulfilled or rejected, but not pending.
*  Using async/await with promises can help you write code that's more intuitive and resembles synchronous code.
*  EX:
  *  async function logIngredients() {
    *  const url = await doSomething();
    *  const res = await fetch(url);
    *  const data = await res.json();
    *  listOfIngredients.push(data);
    *  console.log(listOfIngredients);
     }
* This looks almost exactly like synchronous code, except for the await keywords in front of promises.
* async/await builds on promises- for example, doSomething() is the same function as before, so there's minimal refactoring needed to change from promises to async/await.
* async/await has the same concurrency semantics as normal promise chains. Await within one async function does not stop the entire program, only the parts that depend on its value, so other async jobs can still run while the await is pending.
## Error Handling
* If there's an exception, the browser will look down the chain for catch() handlers or onRejected.
* async function foo() {
  * try {
    * const result = await doSomething();
    * const newResult = await doSomethingElse(result);
    * const finalResult = await doThirdThing(newResult);
    * console.log(`Got the final result: ${finalResult}`);
  * } catch (error) {
    * failureCallback(error);
  * }
* }
* Promises catch all errors, even thrown exceptions and programming errors. This is essential for functional composition of asynchronous operations.
## Nesting
* Nesting is a control structure to limit the scope of catch statements. Specifically, a nested catch only catches failures in its scope and below, not errors higher up in the chain outside the nested scope. When used correctly, this gives greater precision in error recovery.
* async function main() {
  * try {
    * const result = await doSomethingCritical();
  * try {
      * const optionalResult = await doSomethingOptional(result);
      * await doSomethingExtraNice(optionalResult);
   * } catch (e) {
      * // Ignore failures in optional steps and proceed.
   * }
      * await moreCriticalStuff();
  * } catch (e) {
   * console.error(`Critical failure: ${e.message}`);
  * }
* }
* The inner error-silencing catch handler only catches failures from doSomethingOptional() and doSomethingExtraNice(), after which code resumes with moreCriticalStuff(). If doSomethingCritical() fails, the error is caught by the final (outer) catch only, and doesn't get swallowed by the inner catch handler.

# Async Function
* The async function declaration creates a binding of a new async function to a given name. The await keyword is permitted within the function body, enabling asynchronous promise-based behavior to be cleanly written.
* EX:
   * function resolveAfter2Seconds() {
     * return new Promise((resolve) => {
       * setTimeout(() => {
         * resolve('resolved');
       * }, 2000);
     * });
   * }
* async function asyncCall() {
  * console.log('calling');
  * const result = await resolveAfter2Seconds();
  * console.log(result);
  * // Expected output: "resolved"
* }
* asyncCall();
* In the above function, "calling" is output, then two seconds later, "resolved" is output
## Syntax
* async function name(param){
* statements
*  }
## Description
* An async function declaration creates an AsyncFunction object. Each time when an async function is called, it returns a new Promise, which will be resolved with the value returned by the async function, or rehected with an exception uncaught within the async function.
* Await expressions suspend execution of promise-returning functions until the promise to fulfilled or rejected. The resolved value of the promise is treated as the return value of the await expression.
* The await keyword is only valid inside async functions within regular JavaScript code. If you use it outside of an async function's body, you get a SyntaxError.
* Async functions always return a promise; if the return value isn't explicitly a promise, it is implicitly wrapped in a promise.
* async function foo() { return 1} is similar to
* async function foo() { return Promise.resolve(1)}
* Even though the return value of an async function behaves as if it's wrapped in a Promise.resolve, they aren't equivalent.
   * An async function returns a different reference, whereas Promise.resolve returns the same reference if the given value is a promise.
* An async function without an await expression will run synchronously. If there is an await expression inside the function body, however, the async function will always complete asynchronously.
* Code after each await expression can be thought of as existing in a .then callback; a promise chain is progressively constructed with each reentrant step through the function. The return value is the final link in the chain.
* Example Function:
* async function foo() {
  * const result1 = await new Promise((resolve) =>
    * setTimeout(() => resolve("1")),
  * );
  * const result2 = await new Promise((resolve) =>
    * setTimeout(() => resolve("2")),
  * );
* }
* foo();
* Progress moves through the function in three stages...
  1. The first line is executed synchronously, with the await expression configured with the pending promise. Progress through foo is suspended and control is yielded back to the function that called foo.
  2. When the first promise has either been fulfilled or rejected, control moves back into foo. The result of the first promise fulfillment (if it wasn't rejected) is returned from the await expression. Progress continues, and the second await expression is evaluated. Again, progress through foo is suspended and control is yielded.
  3. When the second promise has either been fulfilled or rejected, control re-enters foo. The result of the second promise resolution is returned from the second await expression. Control moves to the return expression (if any).
## Rewriting a Promise Chain with an Async Function
* An API that returns a Promise will result in a promise chain, and it splits the function into many parts. Consider the below code.
* function getProcessedData(url) {
  * return downloadData(url) // returns a promise
    * .catch((e) => downloadFallbackData(url)) // returns a promise
    * .then((v) => processDataInWorker(v)); // returns a promise
* }
* It can be rewritten with a single async function as follows:
* function getProcessedData(url) {
  * return downloadData(url) // returns a promise
    * .catch((e) => downloadFallbackData(url)) // returns a promise
    * .then((v) => processDataInWorker(v)); // returns a promise
* }
