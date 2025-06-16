## Globals in Nodejs:

```bash
__dirname: only accessible in commonjs module
require: function to import modules
process: object gives you info about current process.
```

### Common JS module:
- old/default way
```bash
module.exports = {
 function1,
 function2,
 function3,
};
const funcName = require("./functionPath");
```

### ES Moduling:
```bash
// way 1 - Default Export
function1() {......}
function2() {......}
export default {
  function1
  functon2
}
import function1 from "./path"
import function2 from "./path"

// way 2 - Named Export
export function functionName() {.....}
import { func } from "../path"; //destructuring

// way 3
import defaultExportedFunction, { namedExportedFunction } from "../path";

// way 4
import * as functionName from "./path"
functionName.property (named export)
functionName.default.property (default export)
```

## FS Module in Nodejs :
- to do file I/O (CRUD on file)
- gives 2 variations ---> Promise based function | callback based function
- __dirname : works in commonJs module only

## Async Await:
await:
- operator used to wait for a promise and get its fulfillment value
- can only be used inside of an async function or at top level of module.

```bash
import.meta : object created by host env
exposes context specific meta data to JS module.
contains info like module URL
import.meta.url property ---> full url to module with query parameters
import.meta.resolve property ---> resolves module specific to a url using current module's url as base.

because __dirname doesn't work in ES moduling

process.cwd():
- path of current working directory
- same a __dirname
- cwd = current working directory
- works in ES moduling
```
