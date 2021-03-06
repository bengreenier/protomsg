# protomsg

[![Build Status](https://travis-ci.org/bengreenier/protomsg.svg?branch=master)](https://travis-ci.org/bengreenier/protomsg)

a little module to parse protocol messages over sockjs. just `npm install protomsg`.

# examples

Server:
```
var protomsg = require('protomsg');

// given a sockjs Connection object
var conn;

var emitter = protomsg(conn);
emitter.on("message:test", function (data, seq) {
	// data = "hi", seq = 1
});

// given a sockjs client connects and sends
var fakeMessage = JSON.stringify({id: "test", data: "hi", seq: 1});

// see listener body comment
```

Client:
```
var protomsg = require('protomsg');

var conn = new SockJS("//some/sock/server");

var emitter = protomsg(conn);

emitter.on("message:test", function (data, seq) {
	// data = "hi", seq = 1
});

// given the server we connected to sends
var fakeMessage = JSON.stringify({id: "test", data: "hi", seq: 1});
```

# api

## (sockServerOrClient)

given a sockjs serverside `Connection` object, or a sockjs client side `SockJS` object, emit events if messages using the following structure are found:
```
{
	id: String,
	data: Object,
	seq: Number
}
```
where the event name will be `message:<id>` for any id. Also emit `malformed` if a message is received that doesn't follow that structure.

## events

### malformed

occurs when a message doesn't follow the structure. argment is the raw data of the message.

### message:id

occurs when a message is parsed properly, according to the structure. argument is the parsed `Object`, then the seq.