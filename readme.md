#Real-Time-Web-Applications

##Learning Objectives:
•	Explain the concept of WebSockets
•	Describe the differences between Ajax and WebSockets
•	Explain why we might use WebSockets in our application
•	Implement Socket.io to create an Instant Chat Messaging App

##Ajax Recap

**We have been talking a lot about Ajax throughout this course, why it's relevant and important in the applications we have been developing?**

**PRE-AJAX ERA:**
any user interaction with a webpage or web application required an updated version of the page to be sent to the browser and rendered a full new html page. **This was very slow, and a bad user experience!**

**AJAX ERA:**
(Asynchronous JavaScript and XML) consists of small exchanges of data from the client to the the server, so that the entire web page does not have to be reloaded each time the user makes a change, aka we can send data asynchronously through an HTTP request. **This made our applications faster and vastly improved user experience!**

**POST AJAX ERA(ish):**
A lot of Front End Frameworks today Angular, Backbone, Meteor, etc.. help with making Ajax requests easier for us as developers. However, on applications that use real time data, such as real time gaming, the continuous opening and closing of ajax http requests creates a lot of overhead, and for certain applications, especially those that want rapid responses or real time interactions or display streams of data, it can be a difficult process to simulate

##What are WebSockets?

WebSockets are a TCP based Protocol and API through JavaScript that establishes a single, bi-directional connection between the client and server, allowing full duplex, persistent messages to be instantly distributed.

**Turn & Talk: What do think this means and why might we use it in our applications?**(30 seconds)



![alt tag](https://sdz-upload.s3.amazonaws.com/prod/upload/p3ch1_unsynchronised%20communication%20-%20New%20Page.png)

_Traditional Request-Response Client-Server Model_

![alt tag](https://sdz-upload.s3.amazonaws.com/prod/upload/p3ch1_Communication%20is%20usually%20unsynchronized%2C%20i.e.%20the%20client%20requests%2C%20the%20server%20responds%20-%20New%20Page1.png)

_WebSockets Bi-directional connection_



###WebSockets VS Ajax

- With WebSockets, once a connection is established, it stays open as long as needed. 

- With Ajax, it's is a one-way request that's always initiated by the client, and when the server has sent the response, the connection will close.

- However with WebSockets, both the client and server can initiate and send requests

- Allows a sort of channel that remains open between the client and the server. The browser and the server stay connected to each other and can exchange messages, in one direction and the other, through this channel. Now, the server can decide on its own to send a message to the client

**Benefits to Using WebSockets:**
- Bi-directional messages between Server and Client
- Speed, it's Fast!
- Connection stays open as long as needed
- Useful tool in applications that require real-time data

We will be using one available resource Socket.io, which allows us to easily to utlize a WebSocket in our application. It is available as a module with Node.js and we will be using it in a code exercise below. 

Please note there are MANY RESOURCES available to help us create a WebSocket connection OR approach simulating real time data in our applications, but it entirely depends on our domain and what problem we are trying to solve when choosing a technology to implement in our applications. 

For Example, Google Star Wars Game using uses WebRTC and WebSocket to allow for the real-time communication between the player's desktop and smartphone:
https://lightsaber.withgoogle.com/experience


##We Do: Implement Instant-Chat Messaging App

We are going to be using Nodejs and Express.js along side Socket.io to build a simple chat messaging application. The starter code for this lesson is available here: Please fork and clone this repo down

First thing we need to do when starting a node application with existing dependencies?

```
$ npm install
```
Now that we have all our dependencies install in our Express application, it's time to config socket.io. First, we need to install and save the socket.io module to our package.json file.

```
$ npm install socket.io --save
```

In our main application index.js, we need to require socket.io. In this example we are attaching socket.io to an HTTP server listening on port 4000.

```js
var io = require("socket.io")(http);

io.on("connection", function(socket){
  console.log("Socket.io has been connected!");
});

```
And in our index.html file we need to load our socket.io dependency by adding a script tag.

```html
<script src="/socket.io/socket.io.js"></script>
<script src="http://code.jquery.com/jquery-1.11.1.js"></script>
<script src="js/script.js"></script>
```
Finally, in our script.js, we need to declare a socket variable to call socket.io:

```js
$(document).ready(function(){
  var socket = io();
});
```
Restart your server, and you should see that socket.io has been connected. Great! Now let's grab the content of the message input and have append it to the DOM when a user click's send.

```js
$("button").on("click", function(){
  socket.emit("message", $("#msg").val());
  $("#msg").val("");
  return false;
});

socket.on("message", function(msg){
  $(".messages").append($("<li></li>").text(msg));
});
```
Now we have all the code written on the client side, let's update our back end to receive the message.

```js
io.on("connection", function(socket){
  socket.on('message', function(msg){
    io.emit("message", msg)
    console.log('message:' + msg);
  });
});
```
With socket.io, .emit method is available which emits an event to the socket identified by the string name to all connected clients.

Also, we are sending "message" consistently on the client and server, but please note you could name this anything including "pizza" and as long as it's consistent, it will work the same. Socket.io allows you to do this and create custom events.

Finally, on your browser, you should be able to send messages that appear on your application's webpage! That's it for the code along, feel free to expand on this application and look up additional methods through the documentation.

##Summary

- Web Sockets allow for an open connection that persists between the client and server

- Socket.io is a Node.js module based on WebSockets that allows users to communicate continuously, in real time, with the server when the page is loaded.

- The server and the client communicate by sending each other events through socket.emit() )and listening to the events with socket.on() methods

- As we continue to build more applications that rely on real-time data, it appears WebSockets could be a useful resource or nonetheless, hopefully provoke thought into possible ways of trying to achieve a real time application experience for our users. 

##Additional Resources

Other solutions worth looking into include Firebase, SocketStream, Meteor, Pusher, etc....

http://www.leggetter.co.uk/2013/12/09/choosing-realtime-web-app-tech-stack.html

http://blog.arungupta.me/rest-vs-websocket-comparison-benchmarks/

http://www.dotnetcurry.com/nodejs/1220/create-web-socket-server-nodejs-for-real-time

https://devcenter.heroku.com/articles/node-websockets
