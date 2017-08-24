# Introduction to HTTP and Asynchronous Programming using Node.js
In this assignment we will start to build our UF Directory application. We will use [Node.js](https://en.wikipedia.org/wiki/Node.js) and some of its built in modules to implement a server that provides directory data to clients. 
### What is HTTP? (Make sure to read [this](http://code.tutsplus.com/tutorials/http-the-protocol-every-web-developer-must-know-part-1--net-31177))
HTTP (Hypertext Transfer Protocol) is a **stateless** protocol that allows computers to communicate with each other. We use HTTP to allow our client application (the one users see) to communicate with a server that stores and manipulates data relevant to the user.

HTTP basically boils down to a *request* and a *response*. A client makes a request to either retreive, add, delete, or modify data in some fashion. The host recieves this request, and will provide an appropriate response back to the client. In our case, the server will handle requests for directory listings by responding with listing data in the [JSON](http://stackoverflow.com/questions/383692/what-is-json-and-why-would-i-use-it) format.

### What is Node.js?
Node.js is a Javascript runtime environment built on Google's V8 engine. In other terms, it is a program that interprets Javascript. If I made a file named `hello.js` with the line
```javascript
console.log('Hello, world!');
```
and then typed the command `node hello.js` in my terminal, I should expect to see the text 
`Hello, world!` printed on the screen. 

Node is well known for its ability to run code **asynchronously**.  This means that input and output are non-blocking, and the process of one function does not stop the execution of another. The way this asynchronous code is implemented is through **callback** functions, which are called after a certain process has been completed. The best way to illustrate this is through example. 

This is a simple server that responds to all requests with the text `Request received!`.

```javascript
var http = require('http'); 
var port = 8080; 

var requestHandler = function(request, response) {
  response.end('Request received!');
};

// a server is created, but not started
var server = http.createServer(requestHandler);

// the server is now started, listening for requests on port 8080
server.listen(port, function() {
  //once the server is listening, this callback function is executed
  console.log('Server listening on: http://127.0.0.1:' + port);
});
console.log('Is the server started?');
```
Which log statement do you expect to be printed first? Answer this, then type the command `node simpleServer.js` and see if the results match up with what you were thinking. `Is the server started?` gets printed first is because the call to `server.listen()` is asynchronous in nature. While server.listen() is not finished, the control flow gets passed to the next line of the program. Once server.listen() is finished, it executes the *callback*, defined by the anonymous function: 
```javascript
function() {
    console.log('Server listening on: http://127.0.0.1:' + port);
}
```
Before continuing to the assignment, [these](http://www.theprojectspot.com/tutorial-post/Node-js-for-beginners-part-1-hello-world/2) [two](http://www.theprojectspot.com/tutorial-post/nodejs-for-beginners-callbacks/4) tutorials will help you further understand how Node is used to create servers and the nature of callback functions. 

As you may imagine, the utility of the above server is quite low, since it has no ability to differentiate between requests and respond in the appropriate fashion.

## Assignment
Your objective is to create a server that provides listing data from a JSON file. To accomplish this, you will: 
- use the File System module to load `listings.json` into memory  
- create a request handler with the URL module to send the listing data on a GET request to `localhost:8080/listings`
- use the HTTP module to create a server that makes use of this request handler

We have provided skeleton code that will help guide you in completing this assignment. There is also a file named `server.tests.js` containing unit tests to test your server once completed. 

# Instructions: 

1. Make sure you have [Node.js](https://nodejs.org/en/) installed
2. Clone this repository and then navigate to it on your local machine's terminal 
  See Link for details on how to clone repository - (https://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository)
3. Install the mocha testing framework with the command `npm install -g mocha`
4. Use `npm install` to download all necessary dependencies
5. Implement the server by filling in code blocks found in `server.js`, then test your implementation with the command `mocha server.test.js`. (make sure your server is running before trying to run the tests!)

Some resources you may find useful: 
- [Creating an HTTP server in Node.js](http://www.sitepoint.com/creating-a-http-server-in-node-js/)
- URL [Parsing](https://nodejs.org/api/url.html#url_url_parsing)
- The [HTTP module](https://nodejs.org/api/http.html)
    - [response.writeHead()](https://nodejs.org/api/http.html#http_response_writehead_statuscode_statusmessage_headers)
    - [response.end()](https://nodejs.org/api/http.html#http_response_end_data_encoding_callback)
- The File System's [readFile() method](https://nodejs.org/api/fs.html#fs_fs_readfile_file_options_callback)




