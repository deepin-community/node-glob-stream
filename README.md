<p align="center">
  <a href="http://gulpjs.com">
    <img height="257" width="114" src="https://raw.githubusercontent.com/gulpjs/artwork/master/gulp-2x.png">
  </a>
</p>

# glob-stream

[![NPM version][npm-image]][npm-url] [![Downloads][downloads-image]][npm-url] [![Build Status][ci-image]][ci-url] [![Coveralls Status][coveralls-image]][coveralls-url]

A [Readable Stream][readable-stream-url] interface over [node-glob][node-glob-url].

## Usage

```js
var gs = require('glob-stream');

var readable = gs('./files/**/*.coffee', {
  /* options */
});

var writable =
  /* your WriteableStream */

  readable.pipe(writable);
```

You can pass any combination of glob strings. One caveat is that you cannot **only** pass a negative glob, you must give it at least one positive glob so it knows where to start. If given a non-glob path (also referred to as a singular glob), only one file will be emitted. If given a singular glob and no files match, an error is emitted (see also [`options.allowEmpty`][allow-empty-url]).

## API

### `globStream(globs, options)`

Takes a glob string or an array of glob strings as the first argument and an options object as the second. Returns a stream of objects that contain `cwd`, `base` and `path` properties.

#### Options

##### `options.allowEmpty`

Whether or not to error upon an empty singular glob.

Type: `Boolean`

Default: `false` (error upon no match)

##### `options.dot`

Whether or not to treat dotfiles as regular files. This is passed through to [node-glob][node-glob-url].

Type: `Boolean`

Default: `false`

##### `options.silent`

Whether or not to suppress warnings on stderr from [node-glob][node-glob-url]. This is passed through to [node-glob][node-glob-url].

Type: `Boolean`

Default: `true`

##### `options.cwd`

The current working directory that the glob is resolved against.

Type: `String`

Default: `process.cwd()`

##### `options.root`

The root path that the glob is resolved against.

**Note: This is never passed to [node-glob][node-glob-url] because it is pre-resolved against your paths.**

Type: `String`

Default: `undefined` (use the filesystem root)

##### `options.base`

The absolute segment of the glob path that isn't a glob. This value is attached to each glob object and is useful for relative pathing.

Type: `String`

Default: The absolute path segement before a glob starts (see [glob-parent][glob-parent-url])

##### `options.cwdbase`

Whether or not the `cwd` and `base` should be the same.

Type: `Boolean`

Default: `false`

##### `options.uniqueBy`

Filters stream to remove duplicates based on the string property name or the result of function. When using a function, the function receives the streamed data (objects containing `cwd`, `base`, `path` properties) to compare against.

Type: `String` or `Function`

Default: `'path'`

##### other

Any glob-related options are documented in [node-glob][node-glob-url]. Those options are forwarded verbatim, with the exception of `root` and `ignore`. `root` is pre-resolved and `ignore` is joined with all negative globs.

#### Globbing & Negation

```js
var stream = gs(['./**/*.js', '!./node_modules/**/*']);
```

Globs are executed in order, so negations should follow positive globs. For example:

The following would **not** exclude any files:

```js
gs(['!b*.js', '*.js']);
```

However, this would exclude all files that started with `b`:

```js
gs(['*.js', '!b*.js']);
```

## License

MIT

<!-- prettier-ignore-start -->
[node-glob-url]: https://github.com/isaacs/node-glob
[glob-parent-url]: https://github.com/es128/glob-parent
[allow-empty-url]: #optionsallowempty
[readable-stream-url]: https://nodejs.org/api/stream.html#stream_readable_streams

[downloads-image]: https://img.shields.io/npm/dm/glob-stream.svg?style=flat-square
[npm-url]: https://www.npmjs.com/package/glob-stream
[npm-image]: https://img.shields.io/npm/v/glob-stream.svg?style=flat-square

[ci-url]: https://github.com/gulpjs/glob-stream/actions?query=workflow:dev
[ci-image]: https://img.shields.io/github/workflow/status/gulpjs/glob-stream/dev?style=flat-square

[coveralls-url]: https://coveralls.io/r/gulpjs/glob-stream
[coveralls-image]: https://img.shields.io/coveralls/gulpjs/glob-stream/master.svg?style=flat-square
<!-- prettier-ignore-end -->
