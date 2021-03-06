
# Globals

 As we have learned, node's module system discourages the use of globals. However, node provides a few important globals for us. The first, and probably the most important, is the `process` global which exposes process manipulation (e.g. signalling, exiting, process id (pid), and others). Other globals, such as the `console` object, provide JavaScript functionality common in most browsers.

## console

The `console` object contains several methods which are used to output information to _stdout_ or _stderr_. 

	> console
	{ log: [Function],
	  info: [Function],
	  warn: [Function],
	  error: [Function],
	  dir: [Function],
	  time: [Function],
	  timeEnd: [Function],
	  trace: [Function],
	  assert: [Function] }


Let's take a look at what each method does.

### console.log()

The most frequently used console method is `console.log()`, simply writing to _stdout_ with a line feed (`\n`). Currently aliased as `console.info()`.

	> console.log('wahoo');
	wahoo

	> console.log({ foo: 'bar' });
	{ foo: 'bar' }

### console.error()

Identical to `console.log()`, however writes to _stderr_. Aliased as `console.warn()` as well.

	console.error('database connection failed');

### console.dir()

Utilizes the _sys_ module's `inspect()` method to pretty-print the object to
_stdout_.

	> console.dir({ foo: 'bar' });
	{ foo: 'bar' } 

### console.assert()

Asserts that the given expression is truthy, or throws an exception.  Here, you can also use `console.trace()` and `console.error()` to print a stack trace and a helpful message when the exception is caught.

	try {
	   console.assert( (1 != 2), '1 != 2: Should be true' );
	   console.assert( (1 == 2), '1 == 2: Should be false');
	} catch(e) {
	   console.error('Caught error ' + e);
	   console.trace();
	}

## process

The `process` object is plastered with goodies. In a node console, type `process` and look at what it has to offer. First we will take a look
at some properties that provide information about the node process itself.

### process.version(s)

The `process.version` property contains the node version string, for example "v0.4.0".  The `process.versions` object displays the versions of node, v8, ares, ev, and openssl:

	> process.versions
	{ node: '0.4.0',
	  v8: '3.1.2',
	  ares: '1.7.4',
	  ev: '4.3',
	  openssl: '0.9.8o' }


### process.installPrefix

Exposes the installation prefix, which defaults to "_/usr/local_", as node's binary was installed to "_/usr/local/bin/node_".

### process.execPath

Path to the executable itself, defaults to "_/usr/local/bin/node_".

### process.platform

Exposes a string indicating the platform you are running on, for example "darwin" or "linux".

### process.pid

The process id.

### process.cwd()

Returns the current working directory, for example:

	$ cd /var && node
	> process.cwd()
	'/var'

### process.chdir()

Changes the current working directory to the path passed.

	> process.chdir('lib')
	> process.cwd()
	'/var/lib'

### process.getuid()

Returns the numerical user id of the running process.

### process.setuid()

Sets the effective user id for the running process. This method accepts both a numerical id, as well as a string. For example both `process.setuid(501)`, and `process.setuid('tj')` are valid, where 501 is TJ's _uid_.

### process.getgid()

Returns the numerical group id of the running process.

### process.setgid()

Similar to `process.setuid()` however operates on the group, also accepting a numerical value or string representation. For example `process.setgid(20)` or `process.setgid('www')`.

### process.env

An object containing the user's environment variables, for example:

    { PATH: '/Users/tj/.gem/ruby/1.8/bin:/Users/tj/.nvm/current/bin:/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/usr/X11/bin'
	, PWD: '/Users/tj/ebooks/masteringnode'
	, EDITOR: 'mate'
	, LANG: 'en_CA.UTF-8'
	, SHLVL: '1'
	, HOME: '/Users/tj'
	, LOGNAME: 'tj'
	, DISPLAY: '/tmp/launch-YCkT03/org.x:0'
	, _: '/usr/local/bin/node'
	, OLDPWD: '/Users/tj'
	}

### process.argv

When executing a file with the `node` executable, `process.argv` provides access to the argument vector, the first value being the node executable, second being the filename, and remaining values being the arguments passed.

For example, our source file _./src/process/misc.js_ can be executed by running:

	$ node src/process/misc.js foo bar baz

in which we call `console.dir(process.argv)`, outputting the following:

	[ 'node'
	, '/Users/tj/EBooks/masteringnode/src/process/misc.js'
	, 'foo'
	, 'bar'
	, 'baz'
	]

### process.exit()

The `process.exit()` method is synonymous with the C function `exit()`, in which a exit code > 0 is passed indicating failure, or 0 to indicate success. When invoked the _exit_ event is emitted, allowing a short time for arbitrary processing to occur before `process.reallyExit()` is called with the given status code.

### process.on()

The process itself is an `EventEmitter`, allowing you to do things like listen for uncaught exceptions, via the _uncaughtException_ event:

	// process/exceptions.js
	process.on('uncaughtException', function(err){
	    console.log('got an error: %s', err.message);
	    process.exit(1);
	});

	setTimeout(function(){
	    throw new Error('fail');
	}, 100);

### process.kill()

`process.kill()` method sends the signal passed to the given _pid_, defaulting to **SIGINT**. In our example below we send the **SIGTERM** signal to the same node process to illustrate signal trapping, after which we output "terminating" and exit. Note that our second timeout of 1000 milliseconds is never reached.

	// process/kill.js
	process.on('SIGTERM', function(){
	    console.log('terminating');
	    process.exit(1);
	});

	setTimeout(function(){
	    console.log('sending SIGTERM to process %d', process.pid);
	    process.kill(process.pid, 'SIGTERM');
	}, 500);

	setTimeout(function(){
	    console.log('never called');
	}, 1000);

### process.binding()

Node performs lazy initialization and caching for a number of built-in modules through the `process.binding` function.  If you'd like to poke around in node's source code and view this binding cache implementation, navigate to your node checkout directory and open _./src/node.cc_.  The code for the binding cache starts on like 1749 (of node.js v0.4.0).

Currently, the cacheable process bindings include: built-in modules, `constants`, `io_watcher`, `timer` and `natives`.  To access the constants, instead of using `var constants = require('constants');`, you could lazily bind the constants as it is done in the `fs` module:

	// [node.js source]/lib/fs.js
	var util = require('util');

	var binding = process.binding('fs');
	var constants = process.binding('constants');
	var fs = exports;
	var Stream = require('stream').Stream;
	// ...

Binding in this way allows us to perform the initialization of those modules bound to `process` only once. We can then extend those bindings because they are an exported module like any other `require`.

Another reason for caching the bindings in this way is because these bindings differ across platforms.  If you were to open _node_constants.cc_, you would see conditional includes for _MINGW32_ and `ifdef`s for each constant.  You wouldn't want to hardcode these constants in a module's exports.  If you opened opened the _constants_ module definition, you would see one line:

	// [node.js source]/lib/constants.js
	module.exports = process.binding('constants');
	
After a single require of this module, or call to `process.binding('constants')`, the constants are initialized and cached.  This lazy initialization improves node's startup.

### errno

To access `errno` constants, you'll need to either add the include `var constants = require('constants');` or use the `var constants = process.binding('constants');` method described earlier.

The `constants` object is host of the error numbers, these reference what you would find in C-land, for example `constants.EPERM` represents a permission based error, while `constants.ENOENT` represents a missing file or directory. Typically these are used within bindings to bridge the gap between C++ and JavaScript, however useful for handling exceptions as well as in the http request error object:

    if (err.errno === constants.ENOENT) {
		// Display a 404 "Not Found" page
	} else {
		// Display a 500 "Internal Server Error" page
	}
