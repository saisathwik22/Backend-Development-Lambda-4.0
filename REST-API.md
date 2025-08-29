# `API` : Application Programming Interface
- An API is a `contract` that defines set of rules for one piece of software to interact with another software.

<img width="1179" height="832" alt="image" src="https://github.com/user-attachments/assets/2fb2e7b0-ee31-4531-a3b1-d612735eb1a8" />

## How to implement Network APIs :
- There are recommendations to implement APIs
- If we don't follow these, then nothing will break, unless there is an algorithmic issue in our code.
- But, people might find reading your APIs a bit hard if it doesn't follow these recommendations.
- Not following these recommendations can imapct the readability of code.
- SOAP | `REST` | graphQL | gRPC
- Most of above API recommendations define how to send data between systems, what format we can use to send data, how should the contract look like etc..

# `REST` : Representational State Transfer

## Ideas on which REST API recommendations are decided :

### `Platform Independence` :
- any system should be able to call API of other system independent of what underlying platform is.
- `Example` : System written in Linux OS and System written in Windows OS can have N/W interaction independent of their operating systems.

### `Loose Coupling` :
- 2 Systems should be able to interact without dependence on each other
- `Example` : - Client should not know internal implementations of API.
              - Client shouldn't impact Server | Server shouldn't impact Client
- We will be able to avoid dependence once we hide the logic of internals of API

- To implement REST API we can use any programming language, but our implementation should be done in a way that final API contract follows below recommendations :

### 1. For any data (payload) exchange, we should use JSON
- JSON : JavaScript Object Notation (bunch of key:value pairs | file extension .json | readable | maintainable | easy to parse) 
- while data/request transactions between Client and Server, Server identifies JSON format better.

<img width="866" height="628" alt="image" src="https://github.com/user-attachments/assets/ed77d541-fa3e-4037-8f0f-ca5eb21fdba4" />

### 2. Because REST API involve network interaction we have recommended way of defining URLs of REST API
- REST APIs URLs are designed around `resources`
- `resource` : any kind of data, object or any service representing some real life use case and can be accessed by client.
- In Twitter user, tweet, like, comment are `resources`.
- Signup and SignIn are `actions`.
- Resources are nouns.
- API URLs are designed keeping these nouns in mind not actions.
- `http://twitter.com/tweets/33` | creating tweet is an Action
- http : protocol | twitter.com : host name | tweets/33 : focusing on resource

### 3. With combination of how URL is written and HTTP request method, we can easily identify what API must be doing.

<img width="1060" height="750" alt="image" src="https://github.com/user-attachments/assets/b62e2c51-dd5d-440b-8474-a8d0900bbab9" />


- Action of URL can be defined by combination of `resource + HTTP req method`
- /customers + POST => Creates new customer
- /customers + DELETE => Deletes specific customer

```
/resourceName + POST --- creating a resource
/resourceName + GET --- fetch all records of resource
/resourceName + DELETE --- delete all records of resource
/resourceName + PUT --- bulk update
```

- Variable part of URL starts with `colon (:)`
```
/resourceName/:resourceId
:resourceId => colon represents that this part of url is variable and can take any value.
/resourceName/:resourceId + GET => fetching a particular single record of resource identified by resourceId.
```
`Multi Resource URL`:
- /customers/1/orders : more than 1 resource can be combined in URL

- /customers/1/orders + GET : retrieve all orders for customer 1
- /customers/1/orders + POST : create new order for customer 1
- /customers/1/orders + PUT : bulk update orders for customer 1
- /customers/1/orders + DELETE : remove all orders for customer 1

- /users/:user_id/bookings + POST : create new booking for user with user_id mentioned
- /users/:user_id/orders/:order_id + DELETE : delete order with given order_id of user belonging to given user_id

`POST` : generally used for resource creation.

### 4. Response of REST API should contain numeric representation of status (success/failure) of API - `STATUS CODE`
#### HTTP Response Status Code
- (100 - 199) : Informational response
- (200 - 299) : Successful response
- (300 - 399) : Redirection messages
- (400 - 499) : Client error response
- (500 - 599) : Server error response

- `200 OK` : Successful request (GET, HEAD)
- `201 Created` : request succeeded as a result new resource was created (POST, PUT)
- `202 Accepted` : request received but not acted upon.

- `400 Bad Request` : Server can't process request due to some issue from client.
- `401 Unauthorized` : client must authenticate itself to get requested response.
- `402 Payment Required` : for digital payment systems.
- `403 Forbidden` : client don't have rights to access the content.
- `404 Not Found` : server can't find requested resource.
- `405 Method Not Allowed` : request method is known by server but not supported by target resource.

- `500 Internal Server Error` : server found a situation it doesn't know how to handle | generic error | server can't find apt error code.
- `501 Not Implemented` : request method not supported by server and cant be handled.
- `502 Bad Gateway` : got an invalid response while working as gateway to get response needed to handle request.
- `503 Service Unavailable` : server not ready to handle request.

### 5. REST APIs should be stateless
- it means if multiple HTTP requests are made, then all of these are independent of each other and don't know about each other.
- Every REST API request should be independent of other request.

### 6. To send data in REST API we can use following ways:
- `URL params`: sending info through URL and query params, saved in browser history
```
/customer/1/order : 1 is url param here.
```
- `Query params` : key value pairs embedded in corresponding url | send info about filteration criteria
```
/products?company=samsung&category=mobile
company=samsung & category=mobile are query params here
```
- `Request Body` : not saved in browser history | can handle more complex data to send | can send text, json, xml, but for REST API we send json.
- recommended to version our APIs, version info should be part of URL
- /api/v1/products : v1 is version
- other way to send API version is request headers.

- one of primary motivation behind REST, it should be possible to navigate entire set of resources without requiring prior knowledge of URI scheme.
- `HATEOAS` : Hypertext as the Engine of Application State.
- each HTTP request should return info necessary to find resource related directly to request object through hyperlinks included in response.
- should also provide info that describes operation available in each resource.
