# Arduino Controller

Control your Arduino, and review its state, through NodeJS. This also provides sockets so you can control your Arduino from a remote PC.

## Install (NodeJS)

    npm i arduino-controller

## Install (Javascript)

    <script type="text/javascript" src="arduino-controller.js"></script>

## Usage

This library may be used in two main ways:

1. A way of controlling and handling an Arduino, so you can read and write data from the
   pins using Node, instead of C.
2. A library to facilitate communications between a PC connected to an Arduino, and a remote
   web server.


## Host side

This code would typically be run on a PC.

    const express = require('express');
    const app = express();
    const http = require('http');
    const server = http.createServer(app);
    const io = require('socket.io').listen(server);
    
    const arduino = require('arduino-controller');
    
    server.listen(3000, function(){
      console.log('Express server listening...');
    });
    
    app.use('/', express.static('public'));
    
    var five = require("johnny-five");
    var board = new five.Board({});
    
    board.on("ready", function() {
      var arduino_state = new arduino({controller: five, socketio: io});
    
      io.on('connect', function (socket) {
        console.log('We are connected!');
        arduino_state.onConnectAsHost(socket);
      });
    });

## Client side

The above code would also provide a web server, providing the pages held in the public
direction. This code provides a way connecting the two via sockets.

    <div id="message"></div>
    
    <script type="text/javascript" src="/socket.io/socket.io.js"></script>
    <script type="text/javascript" src="js/arduino-controller.js"></script>
    
    <script>
      var gVars = {};
      var socket = io();
      gVars.arduino = new arduino({board:'uno', socketio:socket});
      gVars.arduino.onConnectAsClient(socket);
      gVars.arduino.onStateChange(function(state) {
        document.getElementById('message').innerHTML = JSON.stringify(state);
      });
    </script>

## Examples

[Arduino monitor](https://github.com/MarquisdeGeek/arduino-controller-monitor)

[Miniature boilerplate](https://github.com/MarquisdeGeek/arduino-controller-mini)
