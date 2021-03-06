# express-sharp

express-sharp adds real-time image processing routes to your express application. Images are processed with [sharp](https://github.com/lovell/sharp), a fast Node.js module for resizing images.

[![Build Status][travis-image]][travis-url]
[![Test Coverage][coveralls-image]][coveralls-url]

## Installation

```sh
$ npm install express-sharp --save
```

See [sharp installation](http://sharp.dimens.io/en/stable/install/) for additional installation instructions.

## Usage

Example *app.js*:

```js
'use strict';

var express = require('express');
var app = express();
var scale = require('express-sharp');

var options = {baseHost: 'mybasehost.com'};
app.use('/my-scale', scale(options));

app.listen(3000));
```

Render `http://mybasehost.com/image.jpg` with 400x400 pixels:

```
GET /my-scale/resize/400?url=%2Fimage.jpg HTTP/1.1
Host: localhost:3000

--> invokes in background:
  GET image.jpg HTTP/1.1
  Host: mybasehost.com
```

Same as above, but with 80% quality, `webp` image type and with progressive enabled:

```
GET /my-scale/resize/400?format=webp&quality=80&progressive=true&url=%2Fimage.jpg HTTP/1.1
Host: localhost:3000
```

## Options

```js
var scale = require('express-sharp');
app.use('/some-path', scale(options));
```

Supported options:

### `baseHost`

Specify the HTTP base host from which images will be requested.

### `cors`

Specify CORS options as described in [cors docs](https://github.com/expressjs/cors). Example:

```js
app.use('/some-path', scale({
  cors: {
    origin: 'http://example.com'
  }
}));
```

If not specified, a `Access-Control-Allow-Origin: *` header is being sent.

## Path and query params

### `format`

Output image format.

Default: output format of the requested image.

Valid values: every valid [sharp output format string](http://sharp.dimens.io/en/stable/api/#toformatformat), i.e. `jpeg`, `gif`, `webp` or `raw`.

### `progressive`

See [sharp docs](http://sharp.dimens.io/en/stable/api/#progressive).

Use `&progressive=true` to enable progressive scan.

### `quality`

See [sharp docs](http://sharp.dimens.io/en/stable/api/#qualityquality).

quality is a Number between 1 and 100.

### `url`

URL/path to original image.

## License

  [MIT](LICENSE)

[travis-image]: https://img.shields.io/travis/pmb0/express-sharp/master.svg
[travis-url]: https://travis-ci.org/pmb0/express-sharp
[coveralls-image]: https://img.shields.io/coveralls/pmb0/express-sharp/master.svg
[coveralls-url]: https://coveralls.io/r/pmb0/express-sharp?branch=master
