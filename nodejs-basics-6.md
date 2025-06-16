## How Event Loop in Nodejs works ?

### Ticks :
- one full iteration of event loop is a TICK
- Every time runtime calls back into JS for an event, we call it a tick.
- a Tick in nodejs refers to execution of a microtask, before the next iteration of loop starts.
- whenever you create Promises, callbacks to those promises goes into microtask queue
- entities that go into microtask queue are called microtasks

```
process.nextTick()
```
- by invoking above, we instruct node to invoke this callback at end of current operation before next tick starts.
- this callback is also a microtask.
- nodejs maintains seperate queue ----> *next Tick queue*

```bash
// Example :
Promise.resolve().then(() => console.log("Resolved promise 1"));
process.nextTick(() => console.log("Process next tick 1"));
setTimeout(() => { console.log("Timer 1 done")}, 0);
process.nextTick(() => console.log("Process next tick 2"));
// Output
Process next tick 1
Process next tick 2
Resolved promise 1
Timer 1 done
```
- before event loop tick starts iteration, pending ticks in nextTick queue are executed.
- after nextTick queue = empty, first event loop iteration starts.
- when stack != empty, event loop is blocked.
![image](https://github.com/user-attachments/assets/2fa8fd07-5049-4d36-81d5-50e3490ddf20)

## Phases
- every timer that is setup using setTimer() and setInterval() is executed first.
- Timer queue get all callbacks of setTimeout and setInterval
- Callbacks in microtask queue are executed after every timer callback in timer queue and even microtask queue we execute nextTick queue callback.
- Nodejs has to do polling to see if the pending i/o task is done?
*poll*
 - retrieves new i/o events.
 - execute i/o related callbacks
 - node will block here when appropriate
- File I/O done ----> cbs of that file i/o added i/o queue thru polling, not automatically.

```bash
fs.readFile(-----------, () => { "callback 3"})
process.nextTick(() => "callback 1")
Promise.resolve().then(() => "callback 2")
for(i = 0----->100000000) { ---- } // blocking code
setTimeout(() => "callback 4", 0)
setImmediate(() => "callback 5")

Execution flow:
callback 1 (nextTick queue)
callback 2 (microtask queue)
callback 4 (timer queue)
callback 5 (check queue)
callback 3 (I/O queue)
```
- file I/O don't add i/o callback automatically to I/O queue, after i/o queue before check queue.
- at last we have close queue which contain callbacks related to closing functions

![image](https://github.com/user-attachments/assets/a069356a-25af-451c-87bc-f43245323ccd)


![image](https://github.com/user-attachments/assets/ba8d7fd0-7273-4973-b97a-490b80890c68)




