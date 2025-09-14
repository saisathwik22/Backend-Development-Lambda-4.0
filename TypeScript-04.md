# Generic Types in Function TS:
### Functions using <T>
- <T> : T represents type
`function function_name<T>(array: T[], x: T): [_, T] {.........} `

```
function testFunction<T>(....) {.....}
// function call using T = number
testFunction<number>(________);

<T,R> : pair ---> functionName<data_type1, data_type2>
<T> : single Entity ----> functionName<data_type>
```

## Stack 
```
class Stack <T> {
  private array : T[]
  constructor() {
    this.array = []
  }
  push (x: T): void{
    this.array.push(x)
  }
  pop() : void {
    this.array.pop()
  }
  top(): T {
    return this.array[this.array.length - 1]
  }
  display(): void {
    console.log(this.array)
  }
}

const st = new Stack<string>();
st.push("abc");    
st.push("def");    
st.push("ghi");    
st.display();      
// [abc(bottom), def, ghi(top)]
```

### Linked List
```
class Node <T> {
  data: T,
  next: node | null,
  constructor (data: T) {
    this.data = data
    this.next = null
  }
}
class LinkedList<T> {
  head : node<T> | null

  constructor() {
    this.head = null
  }
  display() : void {
    let temp : node<T> | null = this.head
    while(temp != null) {
      console.log(temp.data)
      temp = temp.next
    }
  }
  addAtHead(x : T) : void {
    if(this.head == null){
      this.head = new node(x)
      return
    }
    let newNode : node<T> = new node(x)
    newNode.next = this.head
    this.head = newNode
  }
}

let LL = new LinkedList<number>()
LL.addAtHead(1)
LL.addAtHead(2)
LL.addAtHead(22)
LL.addAtHead(4)
LL.display() // 1 -> 2 -> 22 -> 4
```

#### Custom Interface
```
interface custom_interface<T1, T2> {
  property : T1;
  moreProperty: T2;
}
const obj : custom_interface<string, number> = {
  property : "10",
  moreProperty: 20
}
```
