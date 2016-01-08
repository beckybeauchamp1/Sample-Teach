#Real-Time-Web-Applications

##Learning Objectives:
- Introduce and Explain WebSockets
- Highlight the differences between Ajax and WebSockets
- Explain why we might use WebSockets in our application
- Create an Instant Messaging Chat App using Socket.io

##Opening Framing

> We have been talking a lot about Ajax the second half of this course, why is it relevant and important in the applications we have been developing?

**PRE-AJAX:**
- Any user interaction with a webpage or web application required an updated version of the page to be sent to the browser and rendered a full new html page. **This was very slow, and a bad user experience!**

**AJAX:**
- (Asynchronous JavaScript and XML) consists of small exchanges of data from the client to the the server, so that the entire web page does not have to be reloaded each time the user makes a change, aka we can send data asynchronously through an HTTP request. **This made our applications faster and vastly improved user experience!**

**POST AJAX:**
- A lot of Front End Frameworks today like Angular and Backbone help with making Ajax requests easier for us as developers. However, on applications that use real time data, such as real time gaming, the continuous opening and closing of ajax http requests creates a lot of overhead, and for certain applications, especially those that want rapid responses or real time interactions or display streams of data, it can be a difficult process to simulate

##What are WebSockets?

WebSockets are a TCP based Protocol and API that establishes a single, bi-directional connection between the client and server, allowing full duplex, persistent messages to be instantly distributed.

- A Socket could be compared or visualized as a two-way pipe connected between nodes (client and server), where one end is attached to a browser and the other end to a Web server. The WebSocket protocol enables a continuous flow of communication without any need to reload the new page in the browser to reflect information changes received from the server(should sound familiar....)

**Turn & Talk: What do think this means and why might we use it in our applications?**(30 seconds)


![alt tag](https://sdz-upload.s3.amazonaws.com/prod/upload/p3ch1_unsynchronised%20communication%20-%20New%20Page.png)

**_Traditional Request-Response Cycle using HTTP**


![alt tag](https://sdz-upload.s3.amazonaws.com/prod/upload/p3ch1_Communication%20is%20usually%20unsynchronized%2C%20i.e.%20the%20client%20requests%2C%20the%20server%20responds%20-%20New%20Page1.png)

**_WebSockets Bi-directional connection_**



###How Does it Work?

Simplifed Version: Every WebSocket Connection starts with an HTTP GET request from the client to the server ! This is called or refered to as the _"handshake"_. This handshake upgrades the connection from HTTP to the WebSocket protocol. An Example of this might look Something Along These Lines:

```
GET /chat HTTP/1.1
Host: example.com:8000
Upgrade: websocket
Connection: Upgrade
Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==
Sec-WebSocket-Version: 13
```
_This HTTP GET request carries with it a ***Upgrade: websocket*** header and ***Connection: upgrade*** header, telling the server that it wants to begin a WebSocket connection_


##WebSockets VS Ajax

- With WebSockets, once a connection is established, it stays open as long as needed. 

- With Ajax, it's is a one-way request that's always initiated by the client, and when the server has sent the response, the connection will close.

- With WebSockets, both the client and server can initiate and send requests. Now, the server can decide on its own to send a message to the client and essentially eliminates the need for continuous ajax calls or "polling".


##Why would we use WebSockets?

- Allows Bi-directional messages between Server and Client

- Speed, it's Fast!

- Open Connection between Client and Server (Maintains State)

- Useful for Real Time Applications

***Examples:***

- Stock Trader Application: http://demo.kaazing.com/forex/

- Google Star Wars Game: https://lightsaber.withgoogle.com/experience
  - This uses WebRTC and WebSocket to allow for the real-time communication between the player's desktop and smartphone

##We Do: Implement Instant-Messaging Chat App

We will be using one available resource Socket.io, which allows us to easily to utilize a WebSocket in our application. It is available as a module with Node.js, and we will be using it alongside Express.js today

- The starter code for this lesson is available here: https://github.com/beckybeauchamp1/Chat-Messaging-App/tree/master

- Please fork and clone this repo down and checkout the Starter Branch.

```
$ git checkout Starter-Branch
```

> What is the first thing we need to do when starting an express application with existing dependencies?

```
$ npm install
```
Now that we have all our dependencies installed in our Express application, it's time to config socket.io. First, we need to install and save the socket.io module to our package.json file.

```
$ npm install socket.io --save
```

In our main application index.js, we need to require socket.io. In this example we are attaching the socket.io module to an HTTP server listening on port 4000.

```js
var io = require("socket.io")(http);
```

Now, we are going to set an event handler through the .on method in our index.js file that will fire everytime a client socket connects our servers socket.io module. The .on method is acting a a listener for events.

```js
io.on("connection", function(socket){
  console.log("A user has been connected!");
});

```

Furthermore, in our index.html file on the client side we need to load our socket.io dependency by adding a script tag.

```html
<script src="/socket.io/socket.io.js"></script>
<script src="http://code.jquery.com/jquery-1.11.1.js"></script>
<script src="js/script.js"></script>
```
In our script.js file, The first thing we need to do is to create a socket connection to our server's socket.io module.

```js
$(document).ready(function(){
  var socket = io();
});
```
Now, restart your server, and you should see that a user has been connected. 

Great! Now let's grab the content of the user's message and have appended it to the DOM. 

```js
$("button").on("click", function(){
  socket.emit("clientMessage", $("#msg").val());
  $("#msg").val("");
  return false;
});

socket.on("serverMessage", function(msg){
  $(".messages").append($("<li></li>").text(msg));
});
```
First, we are adding an event listener that will be sending the contents of the users message as a string to our server through our socket. We are calling this message "clientMessage". 

Then, we are setting an event handler that will be called when the client receives an event from the server. The event is called “serverMessage” and we can receive the data that has been passed along with the event through our callback function.

Now we have all the code written on the client side, let's update our index.js file to receive the message on the backend:

```js
io.on("connection", function(socket){
  socket.on('clientMessage', function(msg){
    io.emit("serverMessage", msg)
    console.log('message:' + msg);
  });
});
```
Again, With socket.io, **.emit method** is available which emits an event to the socket to all connected clients. We are listening to events at the same time using **.on method**

Also, we are sending "clientMessage" and "serverMessage" consistently on the client and server, but please note you could name this anything, and as long as it's consistent, it will work the same. 

Finally, on your browser, you should be able to send messages that appear on the webpage! That's it for the code along, but feel free to expand on this application and look up additional information through the documentation: https://github.com/socketio/socket.io

##Summary

- Web Sockets allow for an open connection that persists between the client and server

- Socket.io is a Node.js module based on WebSockets that allows users to communicate continuously, in real time, with the server when the page is loaded.

- The server and the client communicate by sending each other events through socket.emit() and listening to the events with socket.on() methods

- There are MANY RESOURCES available to help us create a WebSocket connection OR approach simulating real time data in our applications, but it entirely depends on our domain and what problem we are trying to solve when choosing a technology to implement in our applications. 

- As we continue to build more applications that rely on real-time data, it appears WebSockets could be a useful resource or nonetheless, hopefully thought provoking into possible ways of trying to achieve a real time application experience for our users. 

##Additional Resources

Additional Technologies worth looking into include Firebase, SocketStream, Meteor, Pusher, etc.

Resources for further learning include:

- http://www.leggetter.co.uk/2013/12/09/choosing-realtime-web-app-tech-stack.html

- https://www.smashingmagazine.com/2015/01/why-ajax-isnt-enough/

- http://blog.arungupta.me/rest-vs-websocket-comparison-benchmarks/

- http://www.dotnetcurry.com/nodejs/1220/create-web-socket-server-nodejs-for-real-time

- https://devcenter.heroku.com/articles/node-websockets
