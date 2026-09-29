# @stackline/concat-stream

> writable stream that concatenates strings or binary data and calls a callback with the result.

[![npm version](https://img.shields.io/npm/v/@stackline/concat-stream.svg?style=flat-square)](https://www.npmjs.com/package/@stackline/concat-stream)
[![license](https://img.shields.io/npm/l/@stackline/concat-stream.svg?style=flat-square)](https://github.com/alexandroit/stackline-concat-stream)
[![GitHub repository](https://img.shields.io/badge/GitHub-repository-181717?style=flat-square&logo=github)](https://github.com/alexandroit/stackline-concat-stream)
[![Docs](https://img.shields.io/badge/docs-alexandro.net-0f766e?style=flat-square)](https://alexandro.net/docs/vanilla/concat-stream/)
[![Reddit community](https://img.shields.io/badge/community-r%2FStackline-ff4500?style=flat-square&logo=reddit&logoColor=white)](https://www.reddit.com/r/Stackline/)

**[Documentation](https://alexandro.net/docs/vanilla/concat-stream/)** | **[npm](https://www.npmjs.com/package/@stackline/concat-stream)** | **[Issues](https://github.com/alexandroit/stackline-concat-stream/issues)** | **[Repository](https://github.com/alexandroit/stackline-concat-stream)**

**Current package version:** `1.0.2`

---

## Why this package?

`@stackline/concat-stream` is the Stackline-maintained distribution of `concat-stream@2.0.0`. It is an independent continuation of [concat-stream](https://github.com/maxogden/concat-stream); original authors and licenses remain credited below.

## Compatibility

| Item | Value |
| :--- | :--- |
| Package | `@stackline/concat-stream@1.0.2` |
| API target | `concat-stream@2.0.0` |
| Supported Node.js | `See supported framework requirements` |
| License | `MIT` |
| Main entry | `index.js` |
| Runtime dependencies | `buffer-from, inherits, readable-stream, typedarray` |

## Installation

```bash
npm install @stackline/concat-stream
```

Preserve existing imports and plugin resolution with an npm alias:

```bash
npm install concat-stream@npm:@stackline/concat-stream
```

## Usage and API reference

Writable stream that concatenates all the data from a stream and calls a callback with the result. Use this when you want to collect all the data from a stream into a single buffer.



### description

Streams emit many buffers. If you want to collect all of the buffers, and when the stream ends concatenate all of the buffers together and receive a single buffer then this is the module for you.

Only use this if you know you can fit all of the output of your stream into a single Buffer (e.g. in RAM).

There are also `objectMode` streams that emit things other than Buffers, and you can concatenate these too. See below for details.

## Related

`concat-stream` is part of the [mississippi stream utility collection](https://github.com/maxogden/mississippi) which includes more useful stream modules similar to this one.

### examples

#### Buffers

```js
var fs = require('fs')
var concat = require('@stackline/concat-stream')

var readStream = fs.createReadStream('cat.png')
var concatStream = concat(gotPicture)

readStream.on('error', handleError)
readStream.pipe(concatStream)

function gotPicture(imageBuffer) {
  // imageBuffer is all of `cat.png` as a node.js Buffer
}

function handleError(err) {
  // handle your error appropriately here, e.g.:
  console.error(err) // print the error to STDERR
  process.exit(1) // exit program with non-zero exit code
}

```

#### Arrays

```js
var write = concat(function(data) {})
write.write([1,2,3])
write.write([4,5,6])
write.end()
// data will be [1,2,3,4,5,6] in the above callback
```

#### Uint8Arrays

```js
var write = concat(function(data) {})
var a = new Uint8Array(3)
a[0] = 97; a[1] = 98; a[2] = 99
write.write(a)
write.write('!')
write.end(Buffer.from('!!1'))
```

See `test/` for more examples

### methods

```js
var concat = require('@stackline/concat-stream')
```

## var writable = concat(opts={}, cb)

Return a `writable` stream that will fire `cb(data)` with all of the data that
was written to the stream. Data can be written to `writable` as strings,
Buffers, arrays of byte integers, and Uint8Arrays. 

By default `concat-stream` will give you back the same data type as the type of the first buffer written to the stream. Use `opts.encoding` to set what format `data` should be returned as, e.g. if you if you don't want to rely on the built-in type checking or for some other reason.

* `string` - get a string
* `buffer` - get back a Buffer
* `array` - get an array of byte integers
* `uint8array`, `u8`, `uint8` - get back a Uint8Array
* `object`, get back an array of Objects

If you don't specify an encoding, and the types can't be inferred (e.g. you write things that aren't in the list above), it will try to convert concat them into a `Buffer`.

If nothing is written to `writable` then `data` will be an empty array `[]`.

### error handling

`concat-stream` does not handle errors for you, so you must handle errors on whatever streams you pipe into `concat-stream`. This is a general rule when programming with node.js streams: always handle errors on each and every stream. Since `concat-stream` is not itself a stream it does not emit errors.

We recommend using [`end-of-stream`](https://npmjs.org/end-of-stream) or [`pump`](https://npmjs.org/pump) for writing error tolerant stream code.

### license

MIT LICENSE

## Credits and original authors

- Original project: [concat-stream](https://github.com/maxogden/concat-stream).
- Max Ogden.
- Copyright (c) 2013 Max Ogden.
- Stackline maintenance: [Alexandro Paixao Marques](https://www.linkedin.com/in/aleinfo/) and [Stackline contributors](https://github.com/alexandroit).

Original copyright, license notices and contributor acknowledgements remain part of this distribution. Stackline maintenance does not replace authorship of the original work.

## License

`MIT`. See the license and notice files in the [repository](https://github.com/alexandroit/stackline-concat-stream).

## Community and Links

- [Stackline website](https://alexandro.net/)
- [GitHub projects](https://github.com/alexandroit)
- [npm packages](https://www.npmjs.com/~alex360qc)
- [Reddit community — r/Stackline](https://www.reddit.com/r/Stackline/)
- [Maintainer LinkedIn](https://www.linkedin.com/in/aleinfo/)

Use this repository's issue tracker for reproducible bugs and feature requests. Join r/Stackline for examples, usage questions and release discussions.
