<!-- TITLE/ -->
# Ambi
<!-- /TITLE -->

<!-- BADGES/ -->
[![Build Status](https://secure.travis-ci.org/bevry/ambi.png?branch=master)](http://travis-ci.org/bevry/ambi "Check this project's build status on TravisCI")
[![NPM version](https://badge.fury.io/js/ambi.png)](https://npmjs.org/package/ambi "View this project on NPM")
[![Gittip donate button](http://badgr.co/gittip/bevry.png)](https://www.gittip.com/bevry/ "Donate weekly to this project using Gittip")
[![Flattr donate button](https://raw.github.com/balupton/flattr-buttons/master/badge-89x18.gif)](http://flattr.com/thing/344188/balupton-on-Flattr "Donate monthly to this project using Flattr")
[![PayPayl donate button](https://www.paypalobjects.com/en_AU/i/btn/btn_donate_SM.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=QB8GQPZAH84N6 "Donate once-off to this project using Paypal")
<!-- /BADGES -->

<!-- DESCRIPTION/ -->
Execute a function ambidextrously (normalizes the differences between synchronous and asynchronous functions). Useful for treating synchronous functions as asynchronous functions (like supporting both synchronous and asynchronous event definitions automatically).
<!-- /DESCRIPTION -->

<!-- INSTALL/ -->
## Install

### [Node](http://nodejs.org/), [Browserify](http://browserify.org/)
- Use: `require('ambi')`
- Install: `npm install --save ambi`

### [Ender](http://ender.jit.su/)
- Use: `require('ambi')`
- Install: `ender add ambi`

### [Component](http://github.com/component/component)
- Use: `require('ambi')`
- Install: `component install bevry/ambi`

### [Bower](http://bower.io/)
- Use: `require('ambi')`
- Install: `bower install ambi`
<!-- /INSTALL -->


## Usage

### Example

``` javascript
// Import
var ambi = require('ambi');
var result;

// Sample methods
var syncMethod = function(x,y){
	return x*y;
};
var asyncMethod = function(x,y,next){
	return setTimeout(function(){
		next(null,x*y);
	},0);
};

// Call the synchronous function asynchronously
result = ambi(syncMethod, 5, 2, function(err,result){ // ambi adds support for this asynchronous callback automatically
	console.log(err, result); // null, 10
});
console.log(result); // 10 - just like normal

// Call the asynchronous function asynchronously
result = ambi(asyncMethod, 5, 2, function(err,result){ // ambi doesn't do anything special here
	console.log(err, result); // null, 10
});
console.log(result); // setTimeout - just like normal
```



### Notes

- Ambi accepts the arguments `(method, args...)`
	- `method` is the function to execute
	- `args...` is the arguments to send to the method
		- the last argument is expected to be the completion callback
		- the completion callback is optional, but if defined, is expected to have the signature of `(err, results...)`
- If the method has the same amount of arguments as those ambi received, then we assume it is an asynchronous method and let it handle calling of the completion callback itself
- If the method does not have the same amount of arguments as those ambi received, then we assume it is a synchronous method and we'll call the completion callback ourselves
	- If the synchronous method throws an error or returns an error, we'll try to call the completion callback with a single `err` argument
	- If the synchronous method executes without error, we'll try to call the completion callback with a `err` argument equal to null, and a `result` argument equal to the returned result of the synchronous method
- Ambi can also introspect a different method than the one it fires, by passing `[methodToFire, methodToIntrospect]` as the `method` argument


<!-- HISTORY/ -->
## History
[You can discover the version history inside the `History.md` file.](https://github.com/bevry/ambi/blob/master/History.md#files) file
<!-- /HISTORY -->

<!-- LICENSE/ -->
## License
Licensed under the incredibly [permissive](http://en.wikipedia.org/wiki/Permissive_free_software_licence) [MIT License](http://creativecommons.org/licenses/MIT/)
<br/>Copyright © 2013+ [Bevry Pty Ltd](http://bevry.me)
<br/>Copyright © 2011-2012 [Benjamin Arthur Lupton](http://balupton.com)
<!-- /LICENSE -->
