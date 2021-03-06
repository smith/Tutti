Tutti Terminal and Driver
=========================
This package contains

1. A CLI interface(terminal) for [Tutti](http://tuttijs.com).
2. A library for interfacing with [Tutti](http://tuttijs.com) programatically.

[Tutti](http://tuttijs.com) is an interactive Javascript shell that runs simultaneously on multiple browsers.

CLI interface Usage
=============

    tutti <tutti_room_url>
    
Library
=======
Here is an example of using the Tutti driver to run a test suite using jasmine.

    var log = console.log
    var sys = require('sys')
    var Tutti = require('tutti').Tutti

    function indent(str){
        return str.split('\n').map(function(l){return '  ' + l}).join('\n')
    }

    function display(msg){
        if ('error' in msg){
            log(msg.browser)
            log(indent(msg.error))
        }else if ('console' in msg){
            log(msg.browser)
            log(indent(msg.console))
        }
    }

    Tutti('<tutti_room_url>')
        .on('message', display)
        .reset()
        .load('jasmine.js')
        .load('consoleJasmineReporter.js')
        .load('tests.js')
        .eval('jasmine.getEnv().addReporter(new ConsoleJasmineReporter())')
        .eval('jasmine.getEnv().execute()')
        .wait('console')
        .exit()
        
Driver API
----------

### var tutti = Tutti(room_url)
Creates a new driver and connects to the room specified by the URL.

### tutti.on(message, callback)
Bind to events. Available events are:

- 'connect' - connection established.
- 'message' - a message from the server.
- 'disconnect' - connection terminated.

### tutti.eval(jsOrFunction)
Send a Javascript command to the connected browsers. `jsOrFunction` can either be a string containing Javascript or a function, in which case the function will be executed remotely in the browsers.

### tutti.load(localJSFile, _localJSFile2, ..._)
or
### tutti.load(arrayOfJLocalJSFiles)
Load one or more local Javascript files to be executed by the connected browsers.

### tutti.reset()
Resets the iframe sandbox in the connected browsers. All state will be blown away.

### tutti.wait(messageType, matcher, timeout)
Wait for a type of message before moving on to the next command in the chain.
`messageType` is one of `console`, `error`, `reply`, `announcement`, and `browser`.
`matcher` is a regular expression used to match the content of the message.
`timeout` is the time to wait before giving up and moving on.

### tutti.exit()
Exit this node program.

### tutti.disconnect()
Disconnect with the Tutti server.
    
