# express-dart-sass

Express middleware for [dart-sass](https://github.com/sass/dart-sass).

[![Main CI Workflow](https://github.com/Colbyjdx/express-dart-sass/actions/workflows/ci.yml/badge.svg)](https://github.com/Colbyjdx/express-dart-sass/actions/workflows/ci.yml)
[![npm version](https://badge.fury.io/js/express-dart-sass.svg)](http://badge.fury.io/js/express-dart-sass)

## Install

```bash
npm install express-dart-sass
```

## Usage

Recompile `.scss` or `.sass` files automatically for express based http servers.

### Express example

```javascript
const express = require('express');
const sassMiddleware = require('express-dart-sass');
const path = require('path');
const app = express();
app.use(sassMiddleware({
    /* Options */
    src: __dirname,
    dest: path.join(__dirname, 'public'),
    debug: true,
    outputStyle: 'compressed',
    prefix:  '/prefix'  // Where prefix is at <link rel="stylesheets" href="prefix/style.css"/>
}));
// Note: you must place sass-middleware *before* `express.static` or else it will
// not work.
app.use('/public', express.static(path.join(__dirname, 'public')));
```

### Express with other middleware example

```javascript
const express = require('express');
const sassMiddleware = require('express-dart-sass');
const postcssMiddleware = require('postcss-middleware');
const autoprefixer = require('autoprefixer');
const path = require('path');
const http = require('http');
const app = express();
const destPath = __dirname + '/public';
app.use(sassMiddleware({
    /* Options */
    src: __dirname
  , response: false
  , dest: destPath
  , outputStyle: 'extended'
}));
app.use(postcssMiddleware({
  plugins: [
    /* Plugins */
    autoprefixer({
      /* Options */
    })
  ],
  src: function(req) {
    return path.join(destPath, req.url);
  }
}));

app.listen(3000);
```

### Options

* `src`            - (String) Source directory used to find `.scss` or `.sass` files.

#### Optional configurations

* `beepOnError`    - Enable beep on error, false by default.
* `debug`          - `[true | false]`, false by default. Output debugging information.
* `dest`           - (String) Destination directory used to output `.css` files (when undefined defaults to `src`).
* `error`          - A function to be called when something goes wrong.
* `force`          - `[true | false]`, false by default. Always re-compile.
* `indentedSyntax` - `[true | false]`, false by default. If true compiles files with the `.sass` extension instead of `.scss` in the `src` directory.
* `log`            - `function(severity, key, val, message)`, used to log data instead of the default `console.error`. "severity" matches [Winston](https://www.npmjs.com/package/winston) severity levels.
* `maxAge`         - MaxAge to be passed in Cache-Control header.
* `prefix`         - (String) It will tell the sass middleware that any request file will always be prefixed with `<prefix>` and this prefix should be ignored.
* `response`       - `[true | false]`, true by default. To write output directly to response instead of to a file.
* `root`           - (String) A base path for both source and destination directories.

  For full list of options from original dart-sass project go [here](https://sass-lang.com/documentation/js-api/interfaces/LegacyFileOptions).

### Express example with custom log function

```javascript
const express = require('express');
const sassMiddleware = require('express-dart-sass');
const path = require('path');
const winston = require('winston');
const app = express();
winston.level = 'debug';
app.use(sassMiddleware({
    /* Options */
    src: __dirname,
    dest: path.join(__dirname, 'public'),
    debug: true,
    log: function (severity, key, value) { winston.log(severity, 'express-dart-sass   %s : %s', key, value); }
}));
// Note: you must place sass-middleware *before* `express.static` or else it will
// not work.
app.use(express.static(path.join(__dirname, 'public')));
```

## Contributors

A special thanks to everyone who worked on the original [sass/node-sass-middleware](https://github.com/sass/node-sass-middleware) project. You can find [a full list of those people here](https://github.com/sass/node-sass-middleware/graphs/contributors).

### Building and Testing

```sh
git clone https://github.com/Colbyjdx/express-dart-sass.git
cd express-dart-sass

npm install
npm test
```

### Note on Patches/Pull Requests

* Fork the project.
* Make your feature addition or bug fix.
* Add documentation if necessary.
* Add tests for it. This is important so I don't break it in a future version unintentionally.
* Send a pull request. Bonus points for topic branches.

## Copyright

Copyright (c) 2013+ Andrew Nesbitt. See [LICENSE](https://github.com/Colbyjdx/express-dart-sass/blob/master/LICENSE) for details.
