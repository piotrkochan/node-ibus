# node-ibus

This is a JavaScript implementation of the single wire Ibus protocol found on many BMWs and some of Mini, Land Rover, MG cars. (intended for homebrew use with the RaspberryPi, PC, etc.)

Based on https://github.com/osvathrobi/node-ibus

## Install

```npm install pikey-ibus```


## Usage

Require the packages you want to use.

```
var IbusInterface = require('ibus').IbusInterface;
var IbusDevices = require('ibus').IbusDevices;
```

Configure your device location

```
// config
var device = '/dev/ttyUSB0';

// setup interface
var ibusInterface = new IbusInterface(device);

function init() {
    ibusInterface.startup();
}

// main start
init();
```

Add an event listener on the ibus interface output stream.

```
// events
ibusInterface.on('data', onIbusData);

function onIbusData(data) {
    console.log('[IbusReader]', 'Id: 	  ', data.id);
    console.log('[IbusReader]', 'From: 	  ', IbusDevices.getDeviceName(data.src));
    console.log('[IbusReader]', 'To: 	  ', IbusDevices.getDeviceName(data.dst));
    console.log('[IbusReader]', 'Message: ', data.msg, '\n');
}
```

Cleanup and close the device when exiting.

```
// implementation
process.on('SIGINT', onSignalInt);

function onSignalInt() {
    ibusInterface.shutdown(function() {
        process.exit();
    });
}
``