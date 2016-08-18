# node accelerometer monoaxial

A proof of concept to demostrate any measure captured from a accelerometer attached to a arduino mega using:

- The protocol arduino firmata and a node-red flow on a server to graph it.
- An arduino sketch deployes on your device directly.

![picapp - apple imac_1](https://cloud.githubusercontent.com/assets/1216181/16663437/63d219a8-447c-11e6-98f6-aa01d269c909.png)

The Dashboard UI

![picapp - apple imac_2](https://cloud.githubusercontent.com/assets/1216181/16663470/755fb07c-447c-11e6-8fdf-4b0fa4d0bd41.png)

The system

![picapp - apple imac_2](https://cloud.githubusercontent.com/assets/1216181/16663470/755fb07c-447c-11e6-8fdf-4b0fa4d0bd41.png)

# Hardware:

- [Arduino Mega 2560](https://www.arduino.cc/en/Main/ArduinoBoardMega2560): The MEGA 2560 is designed for more complex projects. With 54 digital I/O pins, 16 analog inputs and a larger space for your sketch.
- [Meas 4610-002-060](http://meas-spec.com/product/tm_product.aspx?id=9902): The Model 4610A is an ultra low-noise accelerometer designed for both static and dynamic measurements. The accelerometer offers integral temperature compensation with dynamic range from ±2 to ±500g. The model 4610A incorporates a gas damped MEMS element with mechanical overload stops for high-g shock protection. 

1) Using Node-Red

The Node-RED flow Designer in server side:

# Infraestructure Techonologies on device

- [node-red-contrib-gpio](https://github.com/monteslu/node-red-contrib-gpio): A set of node-red nodes for connecting to johnny-five IO Plugins.

# Frontend Techonologies on server

- [node-red-contrib-graphs](https://www.npmjs.com/package/node-red-contrib-graphs): A Node-RED graphing package.

# Installation:

Install Arduino firmata on the arduino. Connect the arduino to the PC using USB start Arduino IDE. Configure your Arduino conection to deploy any scketch on your arduino and deploy the StanadardFirmata scketch on your arduino to install the firmata protocol before connect to node-red. Close the Arduino IDE. 

Install Node-Red and Node-Red packages on the server to connect to the arduino device and make a graph
```
npm install
```

- Copy and import this flow from node-red import clipboard to crete the server flow
```
[{"id":"ad18b617.54f588","type":"gpio in","z":"658e1b2a.8c02f4","name":"Accelerometer Monoaxial","state":"ANALOG","samplingInterval":"0.01","pin":"0","board":"e44f4b3e.dd1b28","x":207,"y":153,"wires":[["3da05c7c.03b8a4"]]},{"id":"2117035e.d95b6c","type":"iot-datasource","z":"658e1b2a.8c02f4","name":"Axis Z","tstampField":"","dataField":"","disableDiscover":false,"x":569,"y":98,"wires":[[]]},{"id":"45b36907.77d168","type":"debug","z":"658e1b2a.8c02f4","name":"","active":true,"console":"false","complete":"payload","x":577,"y":184,"wires":[]},{"id":"3da05c7c.03b8a4","type":"function","z":"658e1b2a.8c02f4","name":"Parse","func":"// convert from value to voltage using a 10 bits ADC resolution from Arduino\nV = (5*msg.payload)/1023;\n\n// convert from voltage to gravity\nG = V/3.5;\n\n// extract static acceleration (gravity) from z axis\nA = G - 1;\n\nmsg.payload = {\n  tstamp: new Date().getTime(),\n  data: {\n    z: A\n  }\n}\n\nreturn msg;","outputs":1,"noerr":0,"x":407,"y":153,"wires":[["2117035e.d95b6c","45b36907.77d168"]]},{"id":"e44f4b3e.dd1b28","type":"nodebot","z":"658e1b2a.8c02f4","name":"Arduino Mega","username":"","password":"","boardType":"firmata","serialportName":"/dev/cu.usbmodem641","connectionType":"local","mqttServer":"","socketServer":"","pubTopic":"","subTopic":"","tcpHost":"","tcpPort":"","sparkId":"","sparkToken":"","beanId":"","impId":"","meshbluServer":"https://meshblu.octoblu.com","uuid":"","token":"","sendUuid":""}]
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

You must test that the johnney five node on the flow must connected to your arduino. And show in the debug tab of your node-red instance is writing the measured from your accelerometer sensor.

2) Using a sketch deployed on your arduino device:

In that case you simple deploy the sketch on your arduino and degub the results on your serial tab from your Arduino IDE. In that case the measures are not capture from the server.

# Licenses
The source code is released under Apache 2.0.
