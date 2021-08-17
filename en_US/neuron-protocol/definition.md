# Definition

Neuron supports 3 kinds of Internet protocols, HTTP API, MQTT and Websockets. They are widely used in Internet and web
applications. MQTT protocol will be more specific for communicating with cloud application or IIoT platform. Meanwhile,
the Websockets and HTTP API protocols are used for HMI program or web browsers. These protocols share same JSON data
structure and have same handshake communication processes. These data structure and handshake communication processes
are what we would talk in details in below.

## UUID

All Neuron instances have its own identity UUID (Universal Unique Identity) for IIoT platform. There are two ways to
give UUID to a Neuron. First, the UUID can be automatically generated by Neuron when starting at the first time. Second,
This UUID can be assigned in command line to start Neuron. Neuron will present this UUID to IIoT platform as a unique ID
for the communication token. The UUID **MUST** be 36 characters or less.

For example, UUID: 03c844fc-e381-11e6-b133-000c29158b55

Note: **%UUID%** represents the UUID string of a Neuron.

## MQTT Protocol

Neuron is 100% MQTT compatible. A MQTT broker must be employed in the communication. Generally, any standard MQTT broker
can be used for communication. However, the following brokers have been well tested for Neuron and therefore recommended
for using in project.

1. EMQ [www.emqx.io](http://www.emqx.io)

2. Mosquitto [www.mosquitto.org](http://www.mosquitto.org)

For the port number setup, if you use port number 8880-9 to connect to broker, Neuron will treat the connection as SSL
secured MQTT protocol connection. If using port number 1880-9, the gateway will connect to broker through MQTT protocol
without SSL protection.

The last connection details would be username, password and ClientID. Username and password would be defined by broker.
But _Client ID would be filled with UUID automatically by Neuron._

**_MQTT topic string list_**

Neuron/Broadcast

Neuron/Heartbeat/%UUID%

Neuron/Telemetry/%UUID%

Neuron/Request/%UUID%

Neuron/Response/%UUID%

where %UUID% is a 36 characters UUID string of Neuron.

**_Neuron gateways subscribe topics_**

Neuron/Broadcast

Neuron/Request/%UUID%

**_Neuron gateway publish topics_**

Neuron/Broadcast

Neuron/Heartbeat/%UUID%

Neuron/Telemetry/%UUID%

Neuron/Request/%UUID%

Neuron/Response/%UUID%

**_IIoT Platform subscribe topics_**

Neuron/Broadcast

Neuron/Heartbeat/%UUID%

Neuron/Telemetry/%UUID%

Neuron/Response/%UUID%

**_IIoT Platform publish topics_**

Neuron/Request/%UUID%

During registration process, Neuron will publish repeatedly its UUID to Neuron/Broadcast topic until it gets back the
response from IIoT platform thought the %UUID%/Request topic. If registration success, it will publish the Heartbeat
information on Neuron/Heartbeat/%UUID% topic as well as data will be sent though the Neuron/Telemetry/%UUID% topic to
IIoT platform. Meanwhile, it will subscribe the %UUID%/Request topic to listen command from the IIoT platform. If
request comes, the Neuron would perform the request and send back the answer thought the %UUID%/Response topic. The
topic Neuron/Broadcast are used for Neuron group internal communication.

## Websockets Protocol

Websockets Protocol is usually used for Neuron user interfaces. Of course, you may have other communication purpose. The
Neuron program doesn't deny any possibility from implementation. The only thing you must comply is using below JSON
structure for communication. However, Neuron indeed has provided a web service for users to access the data and
configuration thought the web browser. Type the IP address and Port 7000 in the web browser on the same network. The
port no. MUST be 7000 and have no way to be modified.

**_Websockets connection command in javascript_**

websocket = new WebSocket(wsUri, "datathread");

where wsUri are the IP address of Neuron.

**_Here is an HTML example for Websockets connection_**

```html
<!DOCTYPE html>

<meta charset="utf-8">

<title>WebSocket Test</title>

<script language="javascript" type="text/javascript">

var wsUri = "ws://192.168.1.106:7000/";

var output;

var temp = 0;

function init()

{

output = document.getElementById("output");

testWebSocket();

}

function testWebSocket()

{

websocket = new WebSocket(wsUri, "datathread");

websocket.onopen = function(evt) { onOpen(evt) };

websocket.onclose = function(evt) { onClose(evt) };

websocket.onmessage = function(evt) { onMessage(evt) };

websocket.onerror = function(evt) { onError(evt) };

}

function onOpen(evt)

{

writeToScreen("CONNECTED");

var j={func: 10, wtrm: 'neuron-test-xxx-yyy', name:'root',
pass:'0000'};

var txt = JSON.stringify(j);

doSend(txt);

}

function onClose(evt)

{

writeToScreen("DISCONNECTED");

}

function onMessage(evt)

{

writeToScreen('<span style="color: blue;">RESPONSE: ' + evt.data
+'</span>');

if (temp < 5) {

switch (temp) {

case 1: // read user list

var j1 = {func:13, "wtrm":"DEMO-Neuron-1001"};

break;

case 2: // read user info

var j1 = {func:14, "wtrm":"DEMO-Neuron-1001", name:'joey'};

break;

case 3: // read user list

var j1 = {func:13, "wtrm":"DEMO-Neuron-1001"};

break;

case 4: // read configuration

var j1 = {func:22, "wtrm":"DEMO-Neuron-1001"};

break;

}

var txt1 = JSON.stringify(j1);

doSend(txt1);

}

temp++;

}

function onError(evt)

{

writeToScreen('<span style="color: red;">ERROR:</span> ' +
evt.data);

}

function doSend(message)

{

writeToScreen("SENT: " + message);

websocket.send(message);

}

function writeToScreen(message)

{

var pre = document.createElement("p");

pre.style.wordWrap = "break-word";

pre.innerHTML = message;

output.appendChild(pre);

}

window.addEventListener("load", init, false);


</script>

<h2>WebSocket Test</h2>

<div id="output"></div>
```