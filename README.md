## style-js
#### Javascript style guide for DIY

### Basics
The basic style for DIY, is derived from Google's [Javascript Style Guide](http://google-styleguide.googlecode.com/svn/trunk/javascriptguide.xml) and enforced by [JSHint](http://www.jshint.com/). On top of those basic guidelines, we have a few Node.js specific patterns that we follow:

### Error handling
The short form of this guideline is *"always"*, as in "always handle errors". The standard Node.js idiom for this is as follows:
```javascript
someMethod('foo', function (err, obj) {
    if (err) return callback(err);
    // Go on about your business. Everything is fine.
});
```

### Testing
Automated testing is a must for any module engineered at DIY. The basic pattern is to use [JSHint](http://www.jshint.com/) and [Codebux](https://github.com/substack/codebux) for governance and then simply write short [tap-based](https://github.com/isaacs/node-tap) tests for each piece of functionality. DIY uses [Travis-CI](https://travis-ci.org) for continuous integration testing on both public and private repositories.

For an example of how DIY does testing on large applications, see [api.diy.org](https://github.com/diy/api.diy.org). For an example module which follows DIY's test pattern, see [strainer](https://github.com/thisandagain/strainer).

**Governance Example**
```javascript
var test    = require('tap').test,
    bux     = require('codebux');

bux(__dirname + '/../lib/index.js', function (err, obj) {
    test('governance', function (t) {
        t.equal(err, null, 'Errors should be null');
        t.type(obj, 'number', 'Results should be a number');
        t.ok(obj > 0, 'Total should be greater than zero');
        t.end();
    });
});
```

```javascript
require('jshint-tap-simple').run(__dirname + '/../lib/*.js');
```

**Functional Example**
```javascript
var test    = require('tap').test,
    lib     = require(__dirname + '/../lib/index.js');

lib(function (err, obj) {
    test('functional', function (t) {
        t.equal(err, null, 'Errors should be null');
        t.type(obj, 'object', 'Result should be an object');
        // Assert all the things!
        t.end();
    });
});
```

### Makefiles
Within some more complex modules and most certainly within applications, build and test scripts may be needed. In addition, we often invent small scripts to make working with a specific project easier. In those instances, we use `GNU Make`:
```makefile
test:
    tap test/governance/*.js
    tap test/functional/*.js
    
build:
    lessc public/css/*.less
    
.PHONY test build
```
