# Bandyer Web Core AV

This module provide the Core Audio Video utilities for Bandyer Video using WebRTC.

### Stream

The stream object implements the stream (webcam or screen) acquired by the device of the web client.

Methods:

- initialize(): initialize the Stream object
- initStream(streamConfig, attributes): given a stream config and attributes it init the stream in the browser.
- toggleAudioTrack()
- toggleVideoTrack()
- closeStream()

You will not directly interact with the Stream object. We encourage to use the publisher object.

API doc here: [Stream](https://docs.bandyer.com/Bandyer-Web-Core-AV/classes/stream.html)

### Publisher

The publisher object handles the Webcam o Screen Media Resources of the user.

#### Get local webcam

`.initLocalWebcam(options: GetUserMediaOptions)`

To request access to the client's webcam you should use the initLocalWebcam method with his relative options.

Example:

``` javascript
import {BaseUser, Publisher} from '@bandyer/web-core-av';
// Request client's local webcam access;
const baseUser = BaseUser.initialize({userAlias: 'your_user_alias'});
await Publisher.initialize(baseUser).initLocalWebCam({audio:true, video:true});
```
#### Get local screen

`.initLocalScreen(audio, extensionId?)`

To request access to the client's screen you should use the initLocalScreen function.
You must specify if you want the audio track or not.
The parameter extensionsId is requierd only for GoogleChrome browser.

Example:

``` javascript
import {Publisher} from '@bandyer/web-core-av';
// Request client's screen webcam access;
const baseUser = BaseUser.initialize({userAlias: 'your_user_alias'});
await Publisher.initialize(baseUser).initLocalScreen(true, 'extensiond_id');
```

API doc here: [Publisher](https://docs.bandyer.com/Bandyer-Web-Core-AV/classes/publisher.html)


### Room

Before you can connect to a Room, you need a `Token`. In order to get a Token, you need to interact with the BandyerCommunicationCenter.

### Initialize
```javascript
import { Room } from '@bandyer/web-core-av';
var myRoom = Room.initialize(token, recordOption)
```

You can specify if the room should be recorded or not.
The initialize method returns a Room object. Note that calling the method does not connect to a Room; it creates a JS object which represents a Room.

### Connect to a room
``` javascript
import { Room } from '@bandyer/web-core-av';
try {
	var myRoom = Room.initialize(token);
	await myRoom.connectRoom();
} catch (err) {
	console.log('Room err:', err);
}

```
The connectRoom method returns the Room object if the connection was successfull. Otherwise, it throws an error.

### Disconnect from a room
``` javascript
myRoom.disconnectRoom();
```
To disconnect from a room, call the `disconnectRoom` method.

### Publish stream

Once you have connect to a Room, you can publish the stream of a **Publisher**. First of all, you must initialize a webcam or screen Stream of the **Publisher**.

``` javascript
// first of all, connect to a room
var myRoom = Room.initialize(token);
await myRoom.connectRoom();

// init client's webcam

var myPub = await Publisher.initialize(baseUser)
await myPub.initLocalWebCam({audio:true, video:true});
// publish the Publisher localwebcam
myRoom.publishStream(myPub.localWebcam)

```

### Unpublish stream

You can stop Publisher from streaming a specific Stream to the room by calling the unpublishStream() method of the Room object:

``` javascript
// unpublish the Publisher localwebcam
myRoom.unpublishStream(myPub.localWebcam)

```

#### Room events

##### room-connected

The room object emit the `room-connected` event when you successfully connect to a Room.
A common use case is to subscribe to streams currently published in the room:

``` javascript

var myRoom = Room.initialize(token);
await myRoom.connectRoom();
myRoom.on('room-connected', function(room) {
	// room.streams contains the streams currently published in the room
	const { streams } = roomEvent.streams;
        for (const index in streams) {
            if ({}.hasOwnProperty.call(streams, index)) {
    			const stream = streams[index];
				myRoom.subscribeStream(stream, {audio: true, video: true} );
        	}
    	}
})

```
##### room-disconnected

Emitted when the user has been already disconnected.

``` javascript

var myRoom = Room.initialize(token);
await myRoom.connectRoom();
myRoom.on('room-disconnected', function(room) {
	
})

```

##### room-error

Emitted when it hasn't been possible a successfully connection to the room.

``` javascript

var myRoom = Room.initialize(token);
await myRoom.connectRoom();
myRoom.on('room-error', function(room) {
	
})

```

<!-- 
### Detect stream added
// todo
### Detect stream removed
// todo 
### Room recorded
// todo 
-->


Full doc here: [Room](https://docs.bandyer.com/Bandyer-Web-Core-AV/classes/room.html)