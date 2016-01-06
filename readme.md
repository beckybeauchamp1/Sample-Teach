#Real-Time-Web-Applications

##Learning Objectives:
•	Explain the concept of WebSockets
•	Describe the differences between Ajax and WebSockets
•	List the advantages to using WebSockets in our application
•	Implement Socket.io to create an Instant Chat Messaging App

##Ajax Recap

**We have been talking a lot about Ajax throughout this course, can someone remind to me why it's relevant and important in the applications we have been developing?**

-PRE-AJAX ERA: any user interaction with a webpage or web application required an updated version of the page to be sent to the browser and rendered a full new html page. **This was very slow, and a bad user experience!**

-AJAX ERA: (Asynchronous JavaScript and XML) consists of small exchanges of data from the client to the the server, so that the entire web page does not have to be reloaded each time the user makes a change, aka we can send data asynchronously through an HTTP request. **This made our applications faster and vastly improved user experience!**

-POST AJAX ERA(ish): A lot of Front End Frameworks today Angular, Backbone, Meteor, etc.. help with making Ajax requests easier for us as developers. However, on applications that use real time data, such as real time gaming, the continuous opening and closing of ajax http requests creates a lot of overhead, and for certain applications, especially those that want rapid responses or real time interactions or display streams of data, it can be a difficult process to simulate

## What are WebSockets?

WebSockets are a TCP based Protocol and API through JavaScript that establishes a single, bi-directional connection between the client and server, allowing full duplex, persistent messages to be instantly distributed.

**Turn & Talk to your Neighbor: What do think this means and why might we use it in our applications?**

-Once a connection is established, it stays open as long as needed. Ajax is a one-way request that's always initiated by the client, and when the server has sent the response, the connection will close.

-However with WebSockets, both the client and server can send requests and the connection is kept open.

-As you can imagine, this is pretty powerful when we need to create applications that require real-time data.

![alt tag](https://www.google.com/url?sa=i&rct=j&q=&esrc=s&source=images&cd=&ved=0ahUKEwiS2PbimZTKAhUMRyYKHWHaCRcQjRwIBw&url=http%3A%2F%2Fwww.websocket.org%2Fquantum.html&bvm=bv.110151844,d.dmo&psig=AFQjCNERERsZuzus5hmQjlDPiAfyfeVOng&ust=1452135821733297)

![alt tag](https://sdz-upload.s3.amazonaws.com/prod/upload/p3ch1_unsynchronised%20communication%20-%20New%20Page.png)

![alt tag](https://sdz-upload.s3.amazonaws.com/prod/upload/p3ch1_Communication%20is%20usually%20unsynchronized%2C%20i.e.%20the%20client%20requests%2C%20the%20server%20responds%20-%20New%20Page1.png)


**Main Advantages**
•	Extremely Fast
•	Bi-directional messages between Server and Client
•	Connection stays open as long as needed(State is persisted in Application)

Below, we are going to be using one resource available, Socket.io which allows us to easily establish a WebSocket in our application. Please note there are MANY RESOURCES available to establish WebSockets connections OR approach the problem of creating Real-Time-Web-Applications, but in the context of this lesson we are going to be just focusing on one.

##We Do: Implement Chat Messaging App

We are going to be using Nodejs and Express.js along side Socket.io to build a simple chat messaging application. The starter code for this lesson is available here: Please fork and clone this repo down

Solution Code:
Starter Code:

Please checkout the starter branch to begin building our application:

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
And in our index.html file we need to load our socket.io dependency through adding a script tag.

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

**Can Someone remind me how we go about using jQuery to grab values out of a form**?

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

##Recap

In Summary, we have a lot of resources available as developers to send data over the web asynchronously when creating a real time web application, such as Ajax and WebSockets.

Other solutions worth looking into are Firebase, SailsJS, SocketStream, Meteor Framework built on top of Node.js, and many others. As we continue to build more applications that rely on real-time data, it appears WebSockets could be a useful resource in helping us accomplish our domain.

##Additional Resources

http://www.leggetter.co.uk/2013/12/09/choosing-realtime-web-app-tech-stack.html

http://blog.arungupta.me/rest-vs-websocket-comparison-benchmarks/

http://www.dotnetcurry.com/nodejs/1220/create-web-socket-server-nodejs-for-real-time

https://devcenter.heroku.com/articles/node-websockets
