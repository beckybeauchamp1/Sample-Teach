
#Real Time Web Applications

In this lesson, we will be introducing the concept of WebSockets and comparing them to Ajax in creating Real Time Web Applications

##Learning Objectives:

•	Explain the concept of WebSockets
•	Describe the differences between Ajax and WebSockets
•	List the advantages to using WebSockets in our application
•	Implement Chat Messaging Application using Socket.io

##Ajax Recap

**We have been talking a lot about Ajax throughout this course, can someone remind to me why it's relevant and  important in the applications we have been developing?**

-Before Ajax, any user interaction with a webpage or web application required an updated version of the page to be sent to the browser and rendered a full new html page. Very slow, bad user experience.

-Ajax was introduced-think of Google Maps as an example

-Ajax, stands for Asynchronous JavaScript and XML, and consists of small exchanges of data from the client to the the server, so that the entire web page does not have to be reloaded each time the user makes a change, aka we can send data asynchrously through an HTTP request

-As developers we want a way to make requests to the server from the client side without a full page refresh, Ultimately to make our applications faster and improve user experience.

**But what if our application needs to provide real-time updates? How would we go about updating our view to mirror changes from our backend more seamlessly?**

A lot of Front End Frameworks including BackBone and Angular help with making Ajax requests easier for us as developers, but there is another resource available through HTML5 called WebSockets

##What are Web Sockets?

WebSockets are a TCP based Protocol application that establishes a single, bi-directional connection between the client and server, allowing full duplex, persistent messages to be instantly distributed. Once a connection is established, it stays open as long as needed.

**Can Someone Explain to me again how an Ajax Request Lifecycle** (**hint, starting with a request from the ____**)

Ajax is a one-way request that's always inititated by the client, and when the server has sent the response, the connection will close. However with WebSockets both the client and server can send requests an the connection is kept open. At a high level-WebSockets are built on the TCP protocol, another layer of abstration in the TCP/IP model which is a stateful protocol, we can accomplish this connection.

As you can imagine, this is pretty powerful when we need to create applications that require real-time data.

**Main Advantages**
•	Easily Transfer DataTypes
•	Extremely Fast
•	Bi-directional connection

Below, we are going to be using one resource available, Socket.io which allows us to easily establish a WebSocket in our application. Please note there are MANY resources available to establish WebSockets connections, but in the context of this lesson we are going to be just focusing on one.

We are going to be using Nodejs and Expressjs along side Socket.io to build a simple chat messaging application. The starter code for this lesson is available here: Please fork and clone this repo.

##We Do: Implement Chat Messaging App

The starter code contains all of the dependencies needed for this application. Please clone it down to your local repositories.

**First thing we need to do when starting a node application with existing dependencies**?

```
$ npm install
```
Now that we have all our dependencies install in our Express application, it's time to config socket.io. First, we need to install and save socket.io to our package.json file.

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

And in our index.html file we need to add both jQuery and socket.io script tags:

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
Restart your server, and you should see that socket.io has been connected. Now let's actually grab the content of the message input and have append it to the DOM when a user click's send.

**Can Someone remind me how we go out using jQuery to grab values out of a form**?

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

Finally, at localhost://4000, you should be able to send messages that appear on your application's webpage! That's it for the code along, feel free to expand on this application and look up additional methods through the documentation:

##Recap

In Summary, we have a lot of resources available as developers to send data over the web asynchronously when creating a real time web application, such as Ajax and WebSockets.

Other solutions worth looking into are Firebase, SailsJS, SocketStream, Meteor Framework built on top of Node.js, and many others. As we continue to build more applications that rely on real-time data, it appears WebSockets could be a useful resource in helping us accomplish our domain.

##Additional Resources

http://www.leggetter.co.uk/2013/12/09/choosing-realtime-web-app-tech-stack.html

http://blog.arungupta.me/rest-vs-websocket-comparison-benchmarks/

http://www.dotnetcurry.com/nodejs/1220/create-web-socket-server-nodejs-for-real-time

https://devcenter.heroku.com/articles/node-websockets
