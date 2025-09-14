### Enums or Enumeration
- type that represents named constants

  ```
  enum TicketStatus {
    INITIALISED,
    CANCELLED,
    PENDING,
    CLOSED 
  }
  TicketStatus.INITIALISED = 0;
  TicketStatus.CANCELLED = 1;
  TicketStatus.PENDING = 2;
  TicketStatus.CLOSED = 3;
  ```
- JS equivalent of above Enum
  ```
  var TicketStatus;
  (function (TicketStatus){
    TicketStatus[TicketStatus["INITIALISED"] = 0] = "INITIALISED";
    TicketStatus[TicketStatus["CANCELLED"] = 1] = "CANCELLED";
    TicketStatus[TicketStatus["PENDING"] = 2] = "PENDING";
    TicketStatus[TicketStatus["CLOSED"] = 3] = "CLOSED";
  })(TicketStatus || (TicketStatus = {}));

  TicketStatus.CLOSED //3

  // String comparison is Error prone
  const Ticket = {
    id: "____",
    title: "____",
    status: _____,
  }
  Ticket.status == TicketStatus.INITIALISED // comparable
  ```
- We can have enum to numeric mapping or enum to string mapping
  ```
  const result = {
    name: "Sathwik",
    marks: 98
  }
  // the type of above raw object is inferred as
  // {name:string, marks:number}
  // something like given below
  
  const result : {name: string, marks: number} = {
    name: "Sathwik",
    marks: 98,
  }
  // not allowed because address key not declared int he object
  result.address = "XYX";

  // To solve this we can add optional properties here also.
  const result : {name: string, marks: number, address?: string} = {
    name : "Sathwik",
    marks: 98
  }
  ```
- define common type for auth forms, this will take multiple parameters
  ```
  interface AuthForm {
    name: string,
    submitButtonText: string,
    onResest: (e : any) => void,
    onSubmit: (e: any) => void
  }
  const loginForm : AuthForm = {
    name: "LoginForm",
    submitButtonText: "Login",
    onReset: (e) => {......},
    onSubmit : (e) => {......}
  }
  ```


  ```
  type text = string;
  type String_Array = string[];
  interface Interface_Name {
    [index: number] : number // key: value pair
  }
  ```

- Defining Custom tuples (pair, triplets...)
  ```
  type pair = [number, number]
  type triplet = [number, number, number]
  interface pairInterface {
    first: number,
    second: number
  }
  ```
- Can type and interfaces represent functions ?
  ```
  type logger = (msg: string, errorCode: number) => void;
  interface loggerInterface {
    log: (msg: string, errorCode: number) => void
    //or
    (msg: string, errorCode: number): void
  }
  ```
- defining unions is possible with type but not interfaces
  ```
  type unionOfStringNum = number | string
  interface ComplexNumber {
    real : number,
    imag : number
  }
  add: (num: ComplexNumber) => ComplexNumber
  ```

  ```
  let firstName : string = "sathwik";
  firstName = 10 // non assignable
  let schoolName : "DPS" | "DAV" = "DPS";
  let another : string = "KV";
  schoolName = another // can't assign
  another = schoolName // can assign // DPS
  ```

- `readonly` : re-assignment not possible, once object created, not possible to modify
  ```
  class Product {
    public name
    private price
    readonly category
    constructor {.....}
    display {....}
  }
  ```

