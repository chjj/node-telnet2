# node-telnet2

A fork of [node-telnet][1], implementing env-vars, term-type, and other
miscellaneous features. It is backwardly compatible with node-telnet.

## Example

``` js
var blessed = require('blessed');
var telnet = require('telnet2');

telnet({ tty: true }, function(client) {
  client.on('term', function(terminal) {
    screen.terminal = terminal;
    screen.render();
  });

  client.on('size', function(width, height) {
    client.columns = width;
    client.rows = height;
    client.emit('resize');
  });

  var screen = blessed.screen({
    smartCSR: true,
    input: client,
    output: client,
    terminal: 'xterm-256color',
    fullUnicode: true
  });

  client.on('close', function() {
    if (!screen.destroyed) {
      screen.destroy();
    }
  });

  screen.key(['C-c', 'q'], function(ch, key) {
    screen.destroy();
  });

  screen.on('destroy', function() {
    if (client.writable) {
      client.destroy();
    }
  });

  screen.data.main = blessed.box({
    parent: screen,
    left: 'center',
    top: 'center',
    width: '80%',
    height: '90%',
    border: 'line',
    content: 'Welcome to my server. Here is your own private session.'
  });

  screen.render();
}).listen(2300);
```

## Contribution and License Agreement

If you contribute code to this project, you are implicitly allowing your code
to be distributed under the MIT license. You are also implicitly verifying that
all code is your original work. `</legalese>`

## License

Copyright (c) 2012-2015, Nathan Rajlich. (MIT Licensed)
Copyright (c) 2015, Christopher Jeffrey. (MIT License)

See LICENSE for more info.

[1]: https://github.com/TooTallNate/node-telnet
