## Precise.io Server
A Precise.io Server is an adjusted DataChannel.io Server. All credits go to DataChannel.io for setting up and handling the WebRTC DataChannels and forwarding data, when ever the WebRTC connection fails.

### Changelog
- Added function `emit2` for sending a message to a single client. This uses `rely2` for forwarding over the websocket
- Added optional callback function that is triggered, when the connection to the websocket is ready
- Fixed bug: Rely failed at the `onMessage` function

## DataChannel.io
Cite from [DataChannel.io](https://github.com/marcolanaro/DataChannel.IO) : 

	Datachannel.io is inspired by the amazing socket.io framework and implements a real-time communication using the WebRTC technology.
	Peers are directly connected and datas are exchanged between clients without passing throug the server.

	Socket.io has two purposes:
	* Serve signals between clients needed to coordinate the communication.
	* In case peer-to-peer communication fails or your browser does not support WebRTC, socket.io serve also the data message.

	#### License

	MIT

## How To Set up your Precise.io Server
This installation guide assumes you are using a Debian or Ubuntu Server. It is however not too difficult to adopt these steps on other systems, since node.js works on as good as any platform.

1. create a folder an navigate to it (using cd)

2. install nodejs, npm and redis-server.

	`$ apt-get install nodejs npm redis-server`

3. install modules

	`$ npm install git://github.com/Michael-Schaer/DataChannel.IO.git

4. sometimes the redis module will not be installed in the socket.io folder. If this happens, do the following:	
	
	`$ cd node_modules/socket.io`<br />
	`$ npm install redis`<br />
	`$ cd ../..`

5. run the script
	
	`$ nodejs runDatachannel.js`
	
You should now be running on your local server! If it fails, check your firewall. You can change the port for the DataChannel in the 'runDatachannel.js' script

## How To Use Precise.io Server for other projects (On the clients)

#### Creating a DataChannel Object
	var datachannel = new DataChannel({
		socketServer: 'http://example.com:8080',
		connectedCallback: this.myCallback, // Provide a function that is triggered, when connected to the websocket
		connectedCallbackObject: this, // On what object to call the function
	});
	
	// Or simply
	var datachannel = new DataChannel({
		socketServer: 'http://example.com:8080',
	});

#### Rooms
	// Join a room
	datachannel.join( 'myRoom' );
	
	// Leave a room
	datachannel.leave( 'myRoom' );
	
#### Events
	// Subscribe to an event
	this.datachannel.in( 'myRoom' ).on( 'chat' , function(data) {
	    	console.log(data);
	});
	
	// Send to all clients in a room
	datachannel.in( 'myRoom' ).emit( 'chat' , {text: 'Hi All'} );
	
	// Send to a single client
	datachannel.in( 'myRoom' ).emit2( clientB_Id, 'chat' , {text: 'Hi You'} );
	// Note: You need clientB's id on clientA, in order to use emit2
	
	// Get this clients Id
	datachannel.getId();

## License

MIT
