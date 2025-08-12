# MVC Architecture - Models Views Controllers
- Scalabilty
- Extendability
- Maintainability
- Independent of tech stack
- MVC represent some part of your code like set of functions, classes, structs, interfaces... etc.
- MVC is a way to distribute code based on responsibility.

1. `MODEL (M)` : part of code that we classify as models | models get your business logic implementations (act like chef of hotel)
2. `VIEW (V)` : what user sees (frontend)
3. `CONTROLLER (C)` : part of code which collect request & forward it | Accept request & sending response | act like waiter of hotel

## SRP - Single Responsibility Principle
- 1 part of your code should only have 1 responsibility to handle
- they should be having only 1 reason to change.

```
Ruby is a programming language
Ruby on Rails (RoR) is framework on top of Ruby language
ROR heavily supports MVC architecture
```
`LIBRARIES` 
- customizable
- already prepared snippets of code that solve a particular problem
- reactjs solves problems related to UI | predefined UI components like tables, forms
- hibernate is ORM in world of JAVA | helps to interact with database
- sequelise helps with interaction between Javascript and MySQL

`FRAMEWORKS`
- collection of multiple libraries that helps you to solve a bigger problem.
- no customization.
- example : angularJS, springboot

### Problems of MVC:
- too much simplication of everything, leads to one layer handling many things.
- one piece of code will start doing more than one thing on a granular level.
- nowadays frontend codebase is kept apart from server side code base, MVC doesn't talk about it.

### Client Server architecture
1. collect request
2. process request
3. sending response

- When request comes, it lands upon routing layer in server
#### `Routing Layer`
- decides which set of functions/classes/structs responsible for handling each request based on structure of request, & forwards it to those.
- request from routing layer goes to validation layer.
#### `Validation Layer`
- responsbile for doing request validation
- example, posting empty tweet not allowed
- if valid, forward it to next layer that is handlers/controllers
#### `Handlers / Controllers`
- taking request from upper layer & forwarding it to lower layers
- taking response from lower layer & sending it back.
- forward to service layer
#### `Service Layer`
- responsible for implementing all the core algorithmic & business logic
- example, validating hashtags in a post, checking whether these hastag's exist in DB
- If not creating their entry in DB associating hashtags in DB.
- Service layer donot directly interact with DB when some data access is required.
- Then who interacts?

#### `Repositories | Data Access Object (DAO)`
- set of code which is having only one responsibility, interaction with DB
- they have your actual queries and functions which will be executed on DB server

```
You have a MySQL database. Let's say you can't use MySQL
- Now, Migrate db from MySQL to MongoDB, means change the way of writing queries into MongoDB specific.
- Business logic don't change, but many issues will arise. CHAOS !!!
- Instead of writing DB queries in service layer, write it in seperate layer -> Repository Layer
- If service layer includes DB queries when MySQL queries are being changed to mongoDB queries.
- business logic code may get tampered.

- So database queries in seperate repository avoid direct interaction between service layer and DB
```

<img width="1388" height="705" alt="image" src="https://github.com/user-attachments/assets/a1d538bf-7939-4f40-8a9b-44bcccbacbf5" />

#### `Config Layer`:
-
