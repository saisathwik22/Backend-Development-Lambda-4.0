# TYPESCRIPT
- Types supported
#### Types in TS:(Primitive : can't be altered)
- string | number | boolean | undefined | null | bigint | symbol

### Declaration of variable in TS:
- Type Annotation
  ```
  <let/const/var> <variable_name> : type = <value>;
  ex : let id : number = 5;
  ```
- Type Signature : TS can also automatically infer type just like JS
  ```
  <let/const/var> <variable_name> = <value>
  ex : let id = 5;
  ```

### Difference between JavaScript and TypeScript ?
- Javascript allows `Dynamic Typing` : Type of variable can be changed on the go.
  ```
  let id = 5; // auto infer that id is number
  id = 10; // value changeable
  id = "hello"; // type also can be changed on the go.
  ```
- Typescript : ` NO DYNAMIC TYPING !!!!`
  ```
  let id = 5; // auto infer that id is number
  id = 10; // value can be changed on the go withing same type
  id = "hello"; // error identified because type bool is not assignable to type string.
  // cannot change the type on the go - NO DYNAMIC TYPING !!!
  ```
- can't assign/reassign type of variable once assigned/auto identified by TS
- assigning using `:type` or not, TS will automatically understand the type of variable at 1st assingment and won't allow reassignment

#### Can we place more than one types for a variable in TS ?
- Yes, using Union of Types

### Union Of Types:
- <let/var/const> <variable_name> : data_type1 | data_type2 = <value>;

### For Arrays in TS ?
- for conventional declaration of array in TS:
  ```
  let <variable_name> : type[] = [value1, value2, .....]
  ```
- In JS, arrays are Heterogenous, multiple types allowed in JS arrays
  ```
  arrayInJS = [1, 2, 3, "4", false, 4.532, "hellow", .....]
  ```
- In TS, use `any[]` in place of type[]
  ```
  let heteroArrayTS : any[] = [1, 2, false, "hello", null, ....]
  ```
- We can also use `any` for variable declaration to keep them heterogenous.
  ```
  let x : any = 10;
  x = "100";
  x = false;
  ```
#### Union of Types : Arrays
- let <array_name> : (type1 | type2 | type3)[] = [val1, val2, ....]
- let arr : (number | string | boolean)[] = [1, "100", false];
#### Arrays in form of Typed Tuple
- let array : [number, string, boolean] = [......]
- instead of using (number | string | boolean)[] , use this [number, string, boolean]

