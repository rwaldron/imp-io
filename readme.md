# imp-io

[![Build Status](https://travis-ci.org/rwaldron/imp-io.png?branch=master)](https://travis-ci.org/rwaldron/imp-io)

Imp-io is a Firmata-compatibility IO class for writing node programs that interact with [Electric Imp devices](http://www.electricimp.com/docs/). Imp-io was built at [Bocoup](http://bocoup.com/)

### Getting Started

In order to use the imp-io library, you will need to upload the special
[tyrion](https://github.com/rwaldron/tyrion) agent and device code through Electric Imp's [IDE](https://ide.electricimp.com/login). We recommend you review [Electric Imp's Getting Started](http://www.electricimp.com/docs/gettingstarted/) before continuing.

We also recommend storing your agent ID in a dot file so it can be accessed as a property of `process.env`. Create a file in your home directory called `.imprc` that contains:

```sh
export IMP_AGENT_ID="your agent id"
```

Then add the following to your dot-rc file of choice:

```sh
source ~/.imprc
```


### Blink an Led


The "Hello World" of microcontroller programming:

```js
var Imp = require("imp-io");
var board = new Imp({
  agent: process.env.IMP_AGENT_ID
});

board.on("ready", function() {
  console.log("CONNECTED");
  this.pinMode(9, this.MODES.OUTPUT);

  var byte = 0;

  // This will "blink" the on board led
  setInterval(function() {
    this.digitalWrite(9, (byte ^= 1));
  }.bind(this), 1000);
});
```

### Johnny-Five IO Plugin

Imp-IO can be used as an [IO Plugin](https://github.com/rwaldron/johnny-five/wiki/IO-Plugins) for [Johnny-Five](https://github.com/rwaldron/johnny-five):

```js
var five = require("johnny-five");
var Imp = require("imp-io");

var board = new five.Board({
  io: new Imp({
    agent: process.env.IMP_AGENT_ID
  })
});

board.on("ready", function() {
  var led = new five.Led(9);
  led.blink();
});
```

## License
See LICENSE file.
