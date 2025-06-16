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
**Example :** 
Promise.resolve().then(() => console.log("Resolved promise 1"));
process.nextTick(() => console.log("Process next tick 1"));
setTimeout(() => { console.log("Timer 1 done")}, 0);
process.nextTick(() => console.log("Process next tick 2"));
*Output*
Process next tick 1
Process next tick 2
Resolved promise 1
Timer 1 done
```

![image](https://github.com/user-attachments/assets/2fa8fd07-5049-4d36-81d5-50e3490ddf20)
![image](https://github.com/user-attachments/assets/2fa8fd07-5049-4d36-81d5-50e3490ddf20)


