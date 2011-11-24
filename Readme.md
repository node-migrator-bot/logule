# Logule
Logule is an advanced logging utility for nodejs. It is analogous to `console.log` and can take multiple arguments,
but additionally it prefixes the current time, the log level, and optionally, prefixed namespaces (with optional padding).

Shortcut methods for the log levels are available as `log.error`, `log.warn`, `log.info` and `log.debug`.
These methods are additionally chainable.

## Usage
Here's with a single prefixed namespace:

````javascript
var Logger = require('logule');
var log = new Logger('prefix');
log.error("this is an error message");
log.warn("warning").info("info msg").debug("chained debug");
````
![output!](https://github.com/clux/logule/raw/master/output.png)
### No Namespace
If Logger is instantiated with no constructor arguments, then the output will simply not contain any prefixes and remove one delimiter.
Everything will be aligned automatically.

### Multiple Namespaces
Simply pass in more strings to get more namespaces prefixed.

````javascript
var log = new Logger('BUILD', 'COMPILE');
log.debug('Coffee app.coffee');
````

### Namespace Padding
Padding of the namespace can be done by calling the pad method on the instance with the indentation level you want.

````javascript
var log = new Logger('BUILD').pad(16);
````

Now, the actual log messages will all begin 16 characters after the prefix starts.
If the prefix is longer than the size limit, it will stand out from the crowd.

This is useful when different Logger instances is created in different places with different prefixes, and you want to line up the output.

Note that namespace padding with multiple namespaces is almost impossible to get right (all namespaces pad to an equal length).
If you do enforce superstrict rules, however, the `pad` method will set the pad size for each namespace.

## Passing log around
If using multiple namespaces, then having to write them all out in every module when changing the last is not optimal.

Therefore, it is possible to make a Logger 'subclass' using `sub()`.

````javascript
var log = new logule('BUILD');
var sublog = log.sub('COMPILE');
// pass sublog to the compilation sub-module
// use log in the build hierarchy above compile
````

If the same namespace is fine for another module to use, simply pass log to it.

### Filtering log
If you only want a submodule to be able to log debugs for instance, you can save typing and force this behaviour by calling `get` on log.
This will return the correctly closure bound log method and pass that down.

````javascript
var debug = log.get('debug');
debug("same as log.debug - no other methods accessible through this var");
````

Alternatively, make a copy of the log instance by filtering out the methods you do not want to allow:

````javascript
var sublog = log.remove('debug', 'info');
sublog.warn('works').info('suppressed').error('works!');
log.info('works');
````

## Zalgo
H̸̡̪̯ͨ͊̽̅̾̎Ȩ̬̩̾͛ͪ̈́̀́͘ ̶̧̨̱̹̭̯ͧ̾ͬC̷̙̲̝͖ͭ̏ͥͮ͟Oͮ͏̮̪̝͍M̲̖͊̒ͪͩͬ̚̚͜Ȇ̴̟̟͙̞ͩ͌͝S̨̥̫͎̭ͯ̿̔̀ͅ

````javascript
log.zalgo("all is lost");
````

## Installation

````bash
$ npm install logule
````

## License
MIT-Licensed. See LICENSE file for details.
