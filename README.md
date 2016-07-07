# node accelerometer monoaxial

A proof of concept to demostrate any measure captured from a accelerometer attached to a arduino mega using:

  1) a arduino sketc
  2) arduino firmata protocol and a node-red flow to extract.

The Node-RED flow Designer in server side:

The Dashboard UI

# Hardware:

- [Arduino Mega 2560](https://www.arduino.cc/en/Main/ArduinoBoardMega2560): The MEGA 2560 is designed for more complex projects. With 54 digital I/O pins, 16 analog inputs and a larger space for your sketch.
- [Meas 4610-002-060](http://meas-spec.com/product/tm_product.aspx?id=9902): The Model 4610A is an ultra low-noise accelerometer designed for both static and dynamic measurements. The accelerometer offers integral temperature compensation with dynamic range from ±2 to ±500g. The model 4610A incorporates a gas damped MEMS element with mechanical overload stops for high-g shock protection. 

# Infraestructure Techonologies on device

- [node-red-contrib-gpio](https://github.com/monteslu/node-red-contrib-gpio): A set of node-red nodes for connecting to johnny-five IO Plugins.

# Frontend Techonologies on server

- [node-red-contrib-graphs](https://www.npmjs.com/package/node-red-contrib-graphs): A Node-RED graphing package.

# Installation:

Install Node-Red and Node-Red packages to connect to the arduino device and make a graph
```
npm install
```

- Copy and import this flow from node-red import clipboard to crete the server flow
```
[{"id":"ad18b617.54f588","type":"gpio in","z":"658e1b2a.8c02f4","name":"Accelerometer Monoaxial","state":"ANALOG","samplingInterval":"0.01","pin":"0","board":"e44f4b3e.dd1b28","x":207,"y":153,"wires":[["3da05c7c.03b8a4"]]},{"id":"2117035e.d95b6c","type":"iot-datasource","z":"658e1b2a.8c02f4","name":"Axis Z","tstampField":"","dataField":"","disableDiscover":false,"x":569,"y":98,"wires":[[]]},{"id":"45b36907.77d168","type":"debug","z":"658e1b2a.8c02f4","name":"","active":true,"console":"false","complete":"payload","x":577,"y":184,"wires":[]},{"id":"3da05c7c.03b8a4","type":"function","z":"658e1b2a.8c02f4","name":"Parse","func":"V = (5*msg.payload)/1024;\nG = V/3.5;\nA = G - 1;\n\nmsg.payload = {\n  tstamp: new Date().getTime(),\n  data: {\n    z: A\n  }\n}\n\nreturn msg;","outputs":1,"noerr":0,"x":407,"y":153,"wires":[["2117035e.d95b6c","45b36907.77d168"]]},{"id":"e44f4b3e.dd1b28","type":"nodebot","z":"658e1b2a.8c02f4","name":"Arduino Mega","username":"","password":"","boardType":"firmata","serialportName":"/dev/cu.usbmodem641","connectionType":"local","mqttServer":"","socketServer":"","pubTopic":"","subTopic":"","tcpHost":"","tcpPort":"","sparkId":"","sparkToken":"","beanId":"","impId":"","meshbluServer":"https://meshblu.octoblu.com","uuid":"","token":"","sendUuid":""}]
```

Execute npm to install node-red message router and the UI modules on the server, the package.json has all dependencies:
```
  npm install
```

Start node-red
```
  node node_modules/node-red/red.js
```

Access Node-Red server Web designer
```
  http://localhost:1880/
```

Access Node-Red Dash board UI
```
  http://localhost:1880/dash
```

# Licenses
The source code is released under Apache 2.0.
