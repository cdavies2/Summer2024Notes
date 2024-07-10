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
