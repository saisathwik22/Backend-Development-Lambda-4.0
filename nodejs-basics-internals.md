# Node JS
- Nodejs is an open source, cross platform, runtime env for JavaScript.
- Runtime env provides extra capabilities to JS.
- JS don't have setTimeout(), setInterval() functions, can't access timers.
- Most simple runtime env is BROWSER which provides reading and parsing HTML, DOM features, Networking capabilities to JS.
- fetch() : provided by runtime env, implementation lies with browser.
- Nodejs built with browser capabilities and additional capabilities for JS.
- console.log() also provided by runtime env to JS.
- Nodejs can't access DOM.
- setTimeout(callback funtion, time after which function should be executed)

## cross-platform:
- write code in one type of machine and it can execute in other type of machine.
- machine type can be about OS level, architecture level, 32-bit/64-bit etc.....

## Internals of Nodejs:
- Nodejs gives access of V8 engine to JS, also gives access of libUV, event loop, OS level stuff to JS.
- major runtime env is built using C++.
- JS layer interacts with C++ layer.

![image](https://github.com/user-attachments/assets/03208da5-d2b6-4dcd-85b8-d0de696827d5)


## V8 Engine:
- It's a JS engine which makes running of JS code possible in a machine.
- JS engine contains logic to take JS code written by user and make it executable on the user's machine.
- V8 engine developed by GOOGLE, powers chrome browser and Nodejs, majorly written in C++.
- V8 engine contains components like Parser, Interpreter, Compiler.
- In latest Nodejs version : compiler is Turbofan (earlier: crankshaft), Interpreter: Ignition.

## Parser:
- takes code, tokenizes it and creates Abstract syntax tree.
- tokenization is dividing your code into individual units.
- it makes the code understandable for remaining components of V8 engine.
- AST is created for later set of optimizations, analysis etc...

![image](https://github.com/user-attachments/assets/b547bfe4-56bb-4074-b4e2-d0a22f416fd7)


## Interpreter:
- converts AST to byte code.
- byte code is intermediate code (not final executable code), portable and compact code. Its lighter version of your code.
- creation of byte code makes it more cross-platform capable.
- node allows you to check byte code form.
- below code prints bytecode of code inside filename.js
```
  node --print-bytecode filename.js
```
- ignition converts code into bytecode.
- after conversion it also starts running your byte code.
- byte code is relatively less optimized.
- during running of bytecode, V8 engine starts collecting runtime metrics.
- based on these metrics, V8 engine optimizes the code.
- to optimize the code V8 engine uses Turbofan which is an optimization compiler.
- Turbofan is optimization compiler designed to make effective highly optimized machine code.
- Turbofan can cache some lines of code which are executing again and again.
- Turbofan optimises based on execution metrics
- Turbofan is capable of falling back to previous unoptimized code if optimised code is not useful.

## Compiler Pipeline:
- reading byte code again and again
- reading AST only once.
- Turbofan optimises HOT part of code.
- HOT part of code is something that runs again and again.

  ![image](https://github.com/user-attachments/assets/9a773c26-83b7-4492-b68f-a47f299c2eb0)

## Compilation:
1. JIT : Just In Time.
- compilation during runtime
2. AIT : Ahead In Time.
- compilation before running code.

- V8 engine is component of Nodejs that provides memory aspect to JS code. (access of call stack and memory heap)
- V8 engine also provides garbage collection capabilities.
- V8 engine also uses ORINOCO algorithm.
- ORINOCO does garbage collection cycles, young and old generation segregation.
- if there is an object that survives couple of garbage collection cycles, its classified into old generation
- young generation is newly created memory objects.
- old generation stays in memory for long time.

### Orinoco Algo
- cleans up your memory parallel to JS execution.
- performant garbage collection.
- memory compaction.
- donot intend to block JS execution.
