# api documentation for  [jasmine (v2.5.3)](http://jasmine.github.io/)  [![npm package](https://img.shields.io/npm/v/npmdoc-jasmine.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-jasmine) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-jasmine.svg)](https://travis-ci.org/npmdoc/node-npmdoc-jasmine)
#### Command line jasmine

[![NPM](https://nodei.co/npm/jasmine.png?downloads=true&downloadRank=true&stars=true)](https://www.npmjs.com/package/jasmine)

[![apidoc](https://npmdoc.github.io/node-npmdoc-jasmine/build/screenCapture.buildCi.browser.apidoc.html.png)](https://npmdoc.github.io/node-npmdoc-jasmine/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-jasmine/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-jasmine/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "bin": {
        "jasmine": "./bin/jasmine.js"
    },
    "bugs": {
        "url": "https://github.com/jasmine/jasmine-npm/issues"
    },
    "dependencies": {
        "exit": "^0.1.2",
        "glob": "^7.0.6",
        "jasmine-core": "~2.5.2"
    },
    "description": "Command line jasmine",
    "devDependencies": {
        "grunt": "^0.4.2",
        "grunt-cli": "^0.1.13",
        "grunt-contrib-jshint": "^0.11.0",
        "shelljs": "^0.3.0"
    },
    "directories": {},
    "dist": {
        "shasum": "5441f254e1fc2269deb1dfd93e0e57d565ff4d22",
        "tarball": "https://registry.npmjs.org/jasmine/-/jasmine-2.5.3.tgz"
    },
    "gitHead": "0da2582c4f162c700263b63c3144b99dd56d5015",
    "homepage": "http://jasmine.github.io/",
    "keywords": [
        "test",
        "jasmine",
        "tdd",
        "bdd"
    ],
    "license": "MIT",
    "main": "./lib/jasmine.js",
    "maintainers": [
        {
            "name": "slackersoft"
        },
        {
            "name": "dwfrank"
        },
        {
            "name": "amavisca"
        }
    ],
    "name": "jasmine",
    "optionalDependencies": {},
    "repository": {
        "type": "git",
        "url": "git+https://github.com/jasmine/jasmine-npm.git"
    },
    "scripts": {
        "test": "grunt && ./bin/jasmine.js"
    },
    "version": "2.5.3"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module jasmine](#apidoc.module.jasmine)
1.  [function <span class="apidocSignatureSpan"></span>jasmine (options)](#apidoc.element.jasmine.jasmine)
1.  [function <span class="apidocSignatureSpan">jasmine.</span>ConsoleReporter ()](#apidoc.element.jasmine.ConsoleReporter)
1.  [function <span class="apidocSignatureSpan">jasmine.</span>toString ()](#apidoc.element.jasmine.toString)



# <a name="apidoc.module.jasmine"></a>[module jasmine](#apidoc.module.jasmine)

#### <a name="apidoc.element.jasmine.jasmine"></a>[function <span class="apidocSignatureSpan"></span>jasmine (options)](#apidoc.element.jasmine.jasmine)
- description and source-code
```javascript
function Jasmine(options) {
  options = options || {};
  var jasmineCore = options.jasmineCore || require('jasmine-core');
  this.jasmineCorePath = path.join(jasmineCore.files.path, 'jasmine.js');
  this.jasmine = jasmineCore.boot(jasmineCore);
  this.projectBaseDir = options.projectBaseDir || path.resolve();
  this.printDeprecation = options.printDeprecation || require('./printDeprecation');
  this.specDir = '';
  this.specFiles = [];
  this.helperFiles = [];
  this.env = this.jasmine.getEnv();
  this.reportersCount = 0;
  this.completionReporter = new CompletionReporter();
  this.onCompleteCallbackAdded = false;
  this.exit = exit;
  this.showingColors = true;
  this.reporter = new module.exports.ConsoleReporter();
  this.env.addReporter(this.reporter);
  this.defaultReporterConfigured = false;

  var jasmineRunner = this;
  this.completionReporter.onComplete(function(passed) {
    jasmineRunner.exitCodeCompletion(passed);
  });

  this.coreVersion = function() {
    return jasmineCore.version();
  };
}
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.jasmine.ConsoleReporter"></a>[function <span class="apidocSignatureSpan">jasmine.</span>ConsoleReporter ()](#apidoc.element.jasmine.ConsoleReporter)
- description and source-code
```javascript
function ConsoleReporter() {
  var print = function() {},
    showColors = false,
    timer = noopTimer,
    jasmineCorePath = null,
    printDeprecation = function() {},
    specCount,
    executableSpecCount,
    failureCount,
    failedSpecs = [],
    pendingSpecs = [],
    ansi = {
      green: '\x1B[32m',
      red: '\x1B[31m',
      yellow: '\x1B[33m',
      none: '\x1B[0m'
    },
    failedSuites = [],
    stackFilter = defaultStackFilter,
    onComplete = function() {};

  this.setOptions = function(options) {
    if (options.print) {
      print = options.print;
    }
    showColors = options.showColors || false;
    if (options.timer) {
      timer = options.timer;
    }
    if (options.jasmineCorePath) {
      jasmineCorePath = options.jasmineCorePath;
    }
    if (options.printDeprecation) {
      printDeprecation = options.printDeprecation;
    }
    if (options.stackFilter) {
      stackFilter = options.stackFilter;
    }

    if(options.onComplete) {
      printDeprecation('Passing in an onComplete function to the ConsoleReporter is deprecated.');
      onComplete = options.onComplete;
    }
  };

  this.jasmineStarted = function() {
    specCount = 0;
    executableSpecCount = 0;
    failureCount = 0;
    print('Started');
    printNewline();
    timer.start();
  };

  this.jasmineDone = function(result) {
    printNewline();
    printNewline();
    if(failedSpecs.length > 0) {
      print('Failures:');
    }
    for (var i = 0; i < failedSpecs.length; i++) {
      specFailureDetails(failedSpecs[i], i + 1);
    }

    if (pendingSpecs.length > 0) {
      print("Pending:");
    }
    for(i = 0; i < pendingSpecs.length; i++) {
      pendingSpecDetails(pendingSpecs[i], i + 1);
    }

    if(specCount > 0) {
      printNewline();

      if(executableSpecCount !== specCount) {
        print('Ran ' + executableSpecCount + ' of ' + specCount + plural(' spec', specCount));
        printNewline();
      }
      var specCounts = executableSpecCount + ' ' + plural('spec', executableSpecCount) + ', ' +
        failureCount + ' ' + plural('failure', failureCount);

      if (pendingSpecs.length) {
        specCounts += ', ' + pendingSpecs.length + ' pending ' + plural('spec', pendingSpecs.length);
      }

      print(specCounts);
    } else {
      print('No specs found');
    }

    printNewline();
    var seconds = timer.elapsed() / 1000;
    print('Finished in ' + seconds + ' ' + plural('second', seconds));
    printNewline();

    for(i = 0; i < failedSuites.length; i++) {
      suiteFailureDetails(failedSuites[i]);
    }

    if (result && result.failedExpectations) {
      suiteFailureDetails(result);
    }

    if (result && result.order && result.order.random) {
      print('Randomized with seed ' + result.order.seed);
      printNewline();
    }

    onComplete(failureCount === 0);
  };

  this.specDone = function(result) {
    specCount++;

    if (result.status == 'pending') {
      pendingSpecs.push(result);
      executableSpecCount++;
      print(colored('yellow', '*'));
      return;
    }

    if (result.status == 'passed') {
      executableSpecCount++;
      print(colored('green', '.'));
      return;
    }

    if (result.status == 'failed') {
      failureCount++;
      failedSpecs.push(result);
      executableSpecCount++;
      print(colored('red', 'F'));
    }
  };

  this.suiteDone = function(result) {
    if (result.failedExpectations && result.failedExpectations.length > 0) {
      failureCount++;
      failedSuites.push(result);
    }
  };

  return this;

  function printNewline() {
    print('\n');
  }

  function colored(color, str) {
    return showColors ? (ansi[color] + str + ansi.none) : str;
  }

  function plural(str, count) {
    return count == 1 ? str : str + 's';
  }

  function repeat(thing, times) {
    var arr = [];
    for (var i = 0; i < times; i++) {
      arr.push(thing);
    }
    return arr;
  }

  function indent(str, spaces) {
    var lines = (str || '').split('\n');
    var newArr = [];
    for (var i = 0; i < lines.length; i++) {
      newArr.push(repeat(' ', spac ...
```
- example usage
```shell
n/a
```

#### <a name="apidoc.element.jasmine.toString"></a>[function <span class="apidocSignatureSpan">jasmine.</span>toString ()](#apidoc.element.jasmine.toString)
- description and source-code
```javascript
toString = function () {
    return toString;
}
```
- example usage
```shell
n/a
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
