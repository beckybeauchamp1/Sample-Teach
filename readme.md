
#Creating Real Time Web Applications

#Summary

In this lesson, we will be comparing Ajax and WebSockets in creating Real Time Web Applications

#Learning Objectives:**

•	Recap on the importance of Ajax
•	Recap differences between Asynchronous and Synchronous
•	Explain the concept of Ajax "polling" method
•	Explain what WebSockets are and how they differ from using Ajax
•	List the Advantages to using WebSockets in our application

#Ajax Recap**(2/15)

**Can someone remind me of what Ajax is and why it's important in the applications we have been developing?**

-In order to send data asynchrously through an HTTP request, developers created the XMLHttpRequest object or jQuery's version called Ajax.

-Ajax, stands for Asynchronous JavaScript and XML, and consists of small exchanges of data from the client to the the server, so that the entire web page does not have to be reloaded each time the user makes a change.

-As developers we want a way to make requests to the server from the client side without a full page refresh, Ultimately to make our applications faster and improve user experience.

**But what if our application needs to provide real-time updates? How would we go about updating our view to mirror changes from our backend more seamlessly?**

#What is Ajax Polling?(2/15)

We could use Ajax and “polling” to accomplish this by sending periodic requests for updates to the server, and dynamically manipulating the DOM to reflect those changes. This would essentially create a connection and keep it open for a period of time in which the client can receive data from server. An example is show below using a timeout

```js
(function poll(){
  $.ajax({ url: "server", success: function(data){
    //Update your dashboard gauge
    salesGauge.setValue(data.value);
  }, dataType: "json", complete: poll, timeout: 30000 });
})();

```

A lot of Front End Frameworks help with polling and provide additional methods behind the scenes that help with this. Angular data binding as an example

**Take 1 minute and discuss with your neighbor why this might be a problem in our applications and how some of the Front End Frameworks we learned about help with this solution**

We have many options as developers with strong Front-End Frameworks such as Angular which help making Ajax requests a lot easier or additional APIs to assist in these requests.

There are many different resources available to us as developers for helping us create more real time web applications. Angular has a third party api called Firebase or Pusher which helps to manage this process.


**What are Web Sockets?**(2/5)

WebSockets are a TCP based Protocol application that establishes a single, bi-directional connection between the client and server, allowing full duplex, persistent messages to be instantly distributed. Once a connection is established, it stays open as long as needed. Ajax is a one-way request that's always initated by the client, and when the server has sent the response, the connection will close. However with WebSockets both the client and server can send requests and the connection is kept open. Because WebSockets are built on the TCP protocol, which is a stateful protocol, we can accomplish this connection.

As you can imagine, this is pretty powerful when we need to create applications that require real-time data.

Below, we are going to be using one API available, Socket.io which allows us to easily establish a WebSocket in our application. We are going to be using Nodejs and Expressjs along side Socket.io to build a simple chat messaging application.

**Example of WebSites Using WebSockets(2/3)**

**We Do**

**Chat Messaging App Example**

The starter code contains all of the dependencies needed for this application. Please clone it down to your local repositories.

What's the first thing we need to do after cloning down a node/express application??

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
Restart your server, and you should see that socket.io has been connected. Now let's actually grab the content of the message input and have append it to the DOM when a user click's send. Add this to our script.js file after we have declared socket as a variable.

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

**Take Home**

In Summary, we have a lot of resources available as developers to send data over the web asynchronously when creating a real time web application, such as Ajax and WebSockets.

Other solutions worth looking into are Firebase (that also integrations well as a traditional hosted-service), SailsJS, SocketStream, Meteor(full stack framework built on Node.js) and a number of solutions listed as a no-backend solution. However, as we continue to build more applications that rely on real-time data, it appears WebSockets could be a useful resource in helping us accomplish our domain.

**Additional Resources**

http://www.sitepoint.com/making-http-requests-in-node-js/

http://www.leggetter.co.uk/2013/12/09/choosing-realtime-web-app-tech-stack.html

http://blog.arungupta.me/rest-vs-websocket-comparison-benchmarks/

http://www.dotnetcurry.com/nodejs/1220/create-web-socket-server-nodejs-for-real-time

http://socketo.me/docs/

https://devcenter.heroku.com/articles/node-websockets
