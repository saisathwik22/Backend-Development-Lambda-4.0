# Typescript - Objects, Classes, Functions, Interfaces
- interface does not define how functions work
- Classes contain implementation details of function
- We can use interface to define an existing object, but can't create new object.

  ```
  interfact Product {
    name: string;
    price: number;
    brand: string;
    display(): void;
  }

  let p : Product = {
    name: "Iphone",
    price: 100000,
    brand: "Apple"
  };

  class Car {
    name: string
    constructor(name: string){
      this.name = name;
    }
    display(){
      console.log(this.name);
    }
  }
  ```
- We can create object from class, but can't create new object from interface
- let c1 = new Car("santro"); [Allowed]
- let p1 = new Product(...); [Not Allowed]

### Functions:
- `function function_name() : <return_type> { ......}
- if any type is given, call site can pass other than numbers that can return wrong result.

  ```
  function sum(a : number, b : number) : number {
    return a + b;
  }
  // Optional Parameter
  function sum(a : number, b?: number) : number {
    return a + b;
  }
  // TS starts assuming that b is an optional parameter, because "?" is added (b?: number)
  // If b is not passed, it will be passed as undefined.
  ```
- Every time we run code, code runs normally like JS only.
- But it is just showing us some warnings....
##### So what is TS doing at runtime ?
- TS does not exist in runtime.
- TS will always convert your code to JS, JS will run the code.
- TS is a development tool, it won't do any `code type check` at runtime
- all code type checking will happen at compile time.
- If compilation errors are ignored, code will run like normal JS.

#### Date Object in TS
`const dob = new Date(1998, 11, 3);`
- year : number
- monthIndex : number
- date? : number | undefined
- hours? : number | undefined
- minutes? : number | undefined
- seconds? : number | undefined
- ms? : number | undefined

  ```
  const obj_name = new Date(year, month, date);
  const DOB = new Date(1998, 3, 11);
  DOB.getDate() // 11
  DOB.getFullYear() // 1998
  DOB.getMonth() // 3
  ```
- If you want to get rid of TS type checking, use `any`
- let x : any = value; ||| let x;

