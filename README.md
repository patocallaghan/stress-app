# Stress-app

This app aims to be a stress test for ember-cli.
We will contiue to add scenarios that make it slow, then make it fast again


[issues](https://github.com/ember-cli/stress-app/issues/) are going to track known issues with this app
[ember-cli/issues](https://github.com/ember-cli/ember-cli/issues?q=is%3Aopen+is%3Aissue+label%3Aperformance) is meant for perf issues that in ember-cli but are likely described by a scenario here.

## Current state:


* many app.imports (lets get the number)
* very large bower_components dir (87472k+ files – we hit a pathalogical scenario here that is now largely fixed)
* medium -> large sized app

```
cloc --skip-uniqueness {app,tests}
    2136 text files.
    2136 unique files.
       8 files ignored.

http://cloc.sourceforge.net v 1.64  T=4.05 s (526.9 files/s, 14955.1 lines/s)
-------------------------------------------------------------------------------
Language                     files          blank        comment           code
-------------------------------------------------------------------------------
Javascript                    1308          11115           1976          33679
Handlebars                     823           3209            401          10047
SASS                             2              9              3             96
HTML                             2             10              0             48
-------------------------------------------------------------------------------
SUM:                          2135          14343           2380          43870
-------------------------------------------------------------------------------
```

## Progress so far

progress so far:

* 1.13.0
  *  [initial] Build successful - 47924ms.
  *  [rebuild] Build successful - 19025ms.

* 1.13.8
  *  [initial] Build successful - 38359ms.
  *  [rebuild] Build successful - 8183ms.

* master
  *  [initial] Build successful - 36687ms.
  *  [rebuild] Build successful - 7353ms.

This current state of the stress-app is pretty good respresentation of the current state of medium -> large sized apps. It doesn't yet represent some of the mega sized apps, but that is ok for now. Lets get these numbers down.

* near term target (reasonable near term)
  * [initial warm] Build successful - 10000ms. (this is for warm, cache primed builds and restarts)
  * [rebuild] Build successful - 2500ms. ( feels like a reasonable short term goal 40% of current)

==== more indepth output

```
version: 1.13.0--6d06abacc7
Deprecation warning: sassOptions should be moved to your Brocfile
Livereload server on http://localhost:35729
Serving on http://localhost:5511/

Build successful - 47924ms.

Slowest Trees                                 | Total
----------------------------------------------+---------------------
Babel                                         | 8976ms
Babel                                         | 6825ms
JSHint app- QUnit                             | 6417ms
ES6: App Tree                                 | 3962ms
SourcemapConcat                               | 2633ms

Slowest Trees (cumulative)                    | Total (avg)
----------------------------------------------+---------------------
Babel (13)                                    | 17245ms (1326 ms)
SourcemapConcat (8)                           | 11786ms (1473 ms)
JSHint app- QUnit (1)                         | 6417ms
ES6: App Tree (1)                             | 3962ms

➜  slow-ember-cli-project git:(master) touch app/app.js
➜  slow-ember-cli-project git:(master) file changed app.js

Build successful - 19025ms.

Slowest Trees                                 | Total
----------------------------------------------+---------------------
SourcemapConcat                               | 2489ms
SourcemapConcat                               | 2483ms
SourcemapConcat                               | 2325ms
SourcemapConcat                               | 2286ms
Funnel: App JS Files                          | 2247ms
SourcemapConcat                               | 2206ms
SassCompiler                                  | 1902ms
ES6: App Tree                                 | 1009ms

Slowest Trees (cumulative)                    | Total (avg)
----------------------------------------------+---------------------
SourcemapConcat (8)                           | 11842ms (1480 ms)
Funnel: App JS Files (1)                      | 2247ms
SassCompiler (1)                              | 1902ms
ES6: App Tree (1)                             | 1009ms

```
---

```
➜  slow-ember-cli-project git:(master) ✗ version: 1.13.8--1121141036
Deprecation warning: sassOptions should be moved to your Brocfile
Livereload server on http://localhost:49152
Serving on http://localhost:5511/
Build successful - 38359ms.

Slowest Trees                                 | Total
----------------------------------------------+---------------------
Babel                                         | 8776ms
Babel                                         | 6765ms
JSHint app- QUnit                             | 6638ms
ES6: App Tree                                 | 4096ms
SassCompiler                                  | 3280ms

Slowest Trees (cumulative)                    | Total (avg)
----------------------------------------------+---------------------
Babel (13)                                    | 17149ms (1319 ms)
JSHint app- QUnit (1)                         | 6638ms
ES6: App Tree (1)                             | 4096ms
SassCompiler (1)                              | 3280ms

➜  slow-ember-cli-project git:(master) touch app/app.js
➜  slow-ember-cli-project git:(master) file changed app.js

Build successful - 8183ms.

Slowest Trees                                 | Total
----------------------------------------------+---------------------
SassCompiler                                  | 2295ms
Funnel: App JS Files                          | 1472ms
ES6: App Tree                                 | 1208ms
BroccoliMergeTrees                            | 826ms
Concat: App                                   | 452ms

Slowest Trees (cumulative)                    | Total (avg)
----------------------------------------------+---------------------
SassCompiler (1)                              | 2295ms
Funnel: App JS Files (1)                      | 1472ms
ES6: App Tree (1)                             | 1208ms
BroccoliMergeTrees (15)                       | 863ms (57 ms)
Concat: App (1)                               | 452ms
Babel (13)                                    | 447ms (34 ms)
```

----

```
ember s --port 5511 &
➜  slow-ember-cli-project git:(master) ✗ version: 1.13.8-master-a6e9e4eeaf
Deprecation warning: sassOptions should be moved to your Brocfile
Livereload server on http://localhost:49152
Serving on http://localhost:5511/

Build successful - 37932ms.

Slowest Trees                                 | Total
----------------------------------------------+---------------------
Babel                                         | 8581ms
JSHint app- QUnit                             | 6746ms
Babel                                         | 6562ms
SassCompiler                                  | 4261ms
ES6: App Tree                                 | 3943ms

Slowest Trees (cumulative)                    | Total (avg)
----------------------------------------------+---------------------
Babel (13)                                    | 16553ms (1273 ms)
JSHint app- QUnit (1)                         | 6746ms
SassCompiler (1)                              | 4261ms
ES6: App Tree (1)                             | 3943ms

➜  slow-ember-cli-project git:(master) ✗ touch app/app.js
➜  slow-ember-cli-project git:(master) ✗ file changed app.js

Build successful - 7353ms.

Slowest Trees                                 | Total
----------------------------------------------+---------------------
SassCompiler                                  | 2356ms
Funnel: App JS Files                          | 1752ms
BroccoliMergeTrees                            | 836ms
Concat: App                                   | 585ms

Slowest Trees (cumulative)                    | Total (avg)
----------------------------------------------+---------------------
SassCompiler (1)                              | 2356ms
Funnel: App JS Files (1)                      | 1752ms
BroccoliMergeTrees (15)                       | 874ms (58 ms)
Concat: App (1)                               | 585ms
Babel (13)                                    | 383ms (29 ms)
```

This improvements have been mostly targeted a bower_components \w massive trees causing totally unexpected grief.

Up next:

* persistent filters to improve initial boot for 
  * jshint
  * jscs
  * babel
* alternative approach for CSS plugins (explicitly configure all input trees)
* batch mkdpr creation for broccoli-filter + broccoli-merge-trees -> https://github.com/broccolijs/broccoli-filter/pull/33
* persistentOutput refactors for broccoli-filter + broccoli-merge-trees
* ???

## Prerequisites

You will need the following things properly installed on your computer.

* [Git](http://git-scm.com/)
* [Node.js](http://nodejs.org/) (with NPM)
* [Bower](http://bower.io/)
* [Ember CLI](http://www.ember-cli.com/)
* [PhantomJS](http://phantomjs.org/)

## Installation

* `git clone <repository-url>` this repository
* change into the new directory
* `npm install`
* `bower install`

## Running / Development

* `ember server`
* Visit your app at [http://localhost:4200](http://localhost:4200).

### Code Generators

Make use of the many generators for code, try `ember help generate` for more details

### Running Tests

* `ember test`
* `ember test --server`

### Building

* `ember build` (development)
* `ember build --environment production` (production)

### Deploying

Specify what it takes to deploy your app.

## Further Reading / Useful Links

* [ember.js](http://emberjs.com/)
* [ember-cli](http://www.ember-cli.com/)
* Development Browser Extensions
  * [ember inspector for chrome](https://chrome.google.com/webstore/detail/ember-inspector/bmdblncegkenkacieihfhpjfppoconhi)
  * [ember inspector for firefox](https://addons.mozilla.org/en-US/firefox/addon/ember-inspector/)

