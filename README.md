<!--

@license Apache-2.0

Copyright (c) 2018 The Stdlib Authors.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

-->


<details>
  <summary>
    About stdlib...
  </summary>
  <p>We believe in a future in which the web is a preferred environment for numerical computation. To help realize this future, we've built stdlib. stdlib is a standard library, with an emphasis on numerical and scientific computation, written in JavaScript (and C) for execution in browsers and in Node.js.</p>
  <p>The library is fully decomposable, being architected in such a way that you can swap out and mix and match APIs and functionality to cater to your exact preferences and use cases.</p>
  <p>When you use stdlib, you can be absolutely certain that you are using the most thorough, rigorous, well-written, studied, documented, tested, measured, and high-quality code out there.</p>
  <p>To join us in bringing numerical computing to the web, get started by checking us out on <a href="https://github.com/stdlib-js/stdlib">GitHub</a>, and please consider <a href="https://opencollective.com/stdlib">financially supporting stdlib</a>. We greatly appreciate your continued support!</p>
</details>

# MT19937

[![NPM version][npm-image]][npm-url] [![Build Status][test-image]][test-url] [![Coverage Status][coverage-image]][coverage-url] <!-- [![dependencies][dependencies-image]][dependencies-url] -->

> Create a [readable stream][readable-stream] for a 32-bit [Mersenne Twister][@stdlib/random/base/mt19937] pseudorandom number generator.



<section class="usage">

## Usage

To use in Observable,

```javascript
randomStream = require( 'https://cdn.jsdelivr.net/gh/stdlib-js/random-streams-mt19937@umd/browser.js' )
```

To vendor stdlib functionality and avoid installing dependency trees for Node.js, you can use the UMD server build:

```javascript
var randomStream = require( 'path/to/vendor/umd/random-streams-mt19937/index.js' )
```

To include the bundle in a webpage,

```html
<script type="text/javascript" src="https://cdn.jsdelivr.net/gh/stdlib-js/random-streams-mt19937@umd/browser.js"></script>
```

If no recognized module system is present, access bundle contents via the global scope:

```html
<script type="text/javascript">
(function () {
    window.randomStream;
})();
</script>
```

<a name="random-stream"></a>

#### randomStream( \[options] )

Returns a [readable stream][readable-stream] for a 32-bit [Mersenne Twister][@stdlib/random/base/mt19937] pseudorandom number generator.

```javascript
var inspectStream = require( '@stdlib/streams-node-inspect-sink' );

var iStream;
var stream;

function log( chunk, idx ) {
    console.log( chunk.toString() );
    if ( idx === 10 ) {
        stream.destroy();
    }
}

stream = randomStream();
iStream = inspectStream( log );

stream.pipe( iStream );
```

The function accepts the following `options`:

-   **objectMode**: specifies whether a [stream][stream] should operate in [objectMode][object-mode]. Default: `false`.
-   **encoding**: specifies how `Buffer` objects should be decoded to `strings`. Default: `null`.
-   **highWaterMark**: specifies the maximum number of bytes to store in an internal buffer before ceasing to generate additional pseudorandom numbers.
-   **sep**: separator used to join streamed data. This option is only applicable when a stream is **not** in [objectMode][object-mode]. Default: `'\n'`.
-   **iter**: number of iterations.
-   **normalized**: `boolean` indicating whether to return pseudorandom numbers on the interval `[0,1)`.
-   **seed**: pseudorandom number generator seed.
-   **state**: a [`Uint32Array`][@stdlib/array/uint32] containing pseudorandom number generator state. If provided, the function ignores the `seed` option.
-   **copy**: `boolean` indicating whether to copy a provided pseudorandom number generator state. Setting this option to `false` allows sharing state between two or more pseudorandom number generators and/or streams. Setting this option to `true` ensures that a stream generator has exclusive control over its internal state. Default: `true`.
-   **siter**: number of iterations after which to emit the pseudorandom number generator state. This option is useful when wanting to deterministically capture a stream's underlying PRNG state. Default: `1e308`.

To set [stream][stream] `options`,

```javascript
var opts = {
    'objectMode': true,
    'encoding': 'utf8',
    'highWaterMark': 64
};

var stream = randomStream( opts );
```

By default, the function returns a [stream][stream] which can generate an infinite number of values (i.e., the [stream][stream] will **never** end). To limit the number of generated pseudorandom numbers, set the `iter` option.

```javascript
var inspectStream = require( '@stdlib/streams-node-inspect-sink' );

function log( chunk ) {
    console.log( chunk.toString() );
}

var opts = {
    'iter': 10
};

var stream = randomStream( opts );
var iStream = inspectStream( log );

stream.pipe( iStream );
```

To return pseudorandom numbers on the interval `[0,1)`, set the `normalized` option.

```javascript
var inspectStream = require( '@stdlib/streams-node-inspect-sink' );

function log( chunk ) {
    console.log( chunk.toString() );
}

var opts = {
    'iter': 10,
    'normalized': true
};

var stream = randomStream( opts );
var iStream = inspectStream( log );

stream.pipe( iStream );
```

By default, when not operating in [objectMode][object-mode], a returned [stream][stream] delineates generated pseudorandom numbers using a newline character. To specify an alternative separator, set the `sep` option.

```javascript
var inspectStream = require( '@stdlib/streams-node-inspect-sink' );

function log( chunk ) {
    console.log( chunk.toString() );
}

var opts = {
    'iter': 10,
    'sep': ','
};

var stream = randomStream( opts );
var iStream = inspectStream( log );

stream.pipe( iStream );
```

To seed the underlying pseudorandom number generator, set the `seed` option.

```javascript
var inspectStream = require( '@stdlib/streams-node-inspect-sink' );

function log( v ) {
    console.log( v );
}

var opts = {
    'objectMode': true,
    'iter': 10,
    'seed': 1234
};

var stream = randomStream( opts );

opts = {
    'objectMode': true
};
var iStream = inspectStream( opts, log );

stream.pipe( iStream );
```

To return a [readable stream][readable-stream] with an underlying pseudorandom number generator having a specific initial state, set the `state` option.

```javascript
var inspectStream = require( '@stdlib/streams-node-inspect-sink' );

function log( v ) {
    console.log( v );
}

var opts1 = {
    'objectMode': true,
    'iter': 10
};

var stream = randomStream( opts1 );

var opts2 = {
    'objectMode': true
};
var iStream = inspectStream( opts2, log );

// Stream pseudorandom numbers, thus progressing the underlying generator state:
stream.pipe( iStream );

// Create a new PRNG stream initialized to the last state of the previous stream:
var opts3 = {
    'objectMode': true,
    'iter': 10,
    'state': stream.state
};

stream = randomStream( opts3 );
iStream = inspectStream( opts2, log );

// Stream pseudorandom numbers starting from the last state of the previous stream:
stream.pipe( iStream );
```

##### stream.seed

The value used to seed the underlying pseudorandom number generator.

```javascript
var stream = randomStream();

var seed = stream.seed;
// returns <Uint32Array>
```

##### stream.seedLength

Length of underlying pseudorandom number generator seed.

```javascript
var stream = randomStream();

var len = stream.seedLength;
// returns <number>
```

##### stream.state

Writable property for getting and setting the underlying pseudorandom number generator state.

```javascript
var stream = randomStream();

var state = stream.state;
// returns <Uint32Array>
```

##### stream.stateLength

Length of underlying pseudorandom number generator state.

```javascript
var stream = randomStream();

var len = stream.stateLength;
// returns <number>
```

##### stream.byteLength

Size (in bytes) of underlying pseudorandom number generator state.

```javascript
var stream = randomStream();

var sz = stream.byteLength;
// returns <number>
```

* * *

#### randomStream.factory( \[options] )

Returns a `function` for creating [readable streams][readable-stream] which generate pseudorandom numbers via a 32-bit [Mersenne Twister][@stdlib/random/base/mt19937] pseudorandom number generator.

```javascript
var opts = {
    'objectMode': true,
    'encoding': 'utf8',
    'highWaterMark': 64
};

var createStream = randomStream.factory( opts );
```

The method accepts the same `options` as [`randomStream()`](#random-stream).

* * *

#### randomStream.objectMode( \[options] )

This method is a convenience function to create [streams][stream] which **always** operate in [objectMode][object-mode].

```javascript
var inspectStream = require( '@stdlib/streams-node-inspect-sink' );

function log( v ) {
    console.log( v );
}

var opts = {
    'iter': 10
};
var stream = randomStream.objectMode( opts );

opts = {
    'objectMode': true
};
var iStream = inspectStream( opts, log );

stream.pipe( iStream );
```

This method accepts the same `options` as [`randomStream()`](#random-stream); however, the method will **always** override the [`objectMode`][object-mode] option in `options`.

* * *

### Events

In addition to the standard [readable stream][readable-stream] events, the following events are supported...

#### 'state'

Emitted after internally generating `siter` pseudorandom numbers.

```javascript
var opts = {
    'siter': 10 // emit the PRNG state every 10 pseudorandom numbers
};

var stream = randomStream( opts );

stream.on( 'state', onState );

function onState( state ) {
    // Do something with the emitted state, such as save to file...
}
```

</section>

<!-- /.usage -->

* * *

<section class="notes">

## Notes

-   [Mersenne Twister][@stdlib/random/base/mt19937] is **not** a cryptographically secure PRNG, as the PRNG is based on a linear recursion. Any pseudorandom number sequence generated by a linear recursion is **insecure**, due to the fact that one can predict future generated outputs by observing a sufficiently long subsequence of generated values.
-   The underlying PRNG has a period of `2^19937 - 1`.
-   If PRNG state is "shared" (meaning a state array was provided during stream creation and **not** copied) and one sets the generator state to a state array having a different length, the underlying PRNG does **not** update the existing shared state and, instead, points to the newly provided state array. In order to synchronize PRNG output according to the new shared state array, the state array for **each** relevant PRNG must be **explicitly** set.
-   If PRNG state is "shared" and one sets the generator state to a state array of the same length, the PRNG state is updated (along with the state of all other PRNGs sharing the PRNG's state array).
-   In order to capture the PRNG state after a specific number of generated pseudorandom numbers, regardless of internal stream buffering, use the `siter` option in conjunction with a `state` event listener. Attempting to capture the underlying PRNG state after **reading** generated numbers is **not** likely to give expected results, as internal stream buffering will mean more values have been generated than have been read. Thus, the state returned by the `state` property will likely reflect a future PRNG state from the perspective of downstream consumers.

</section>

<!-- /.notes -->

* * *

<section class="examples">

## Examples

<!-- eslint no-undef: "error" -->

```html
<!DOCTYPE html>
<html lang="en">
<body>
<script type="text/javascript" src="https://cdn.jsdelivr.net/gh/stdlib-js/streams-node-inspect-sink@umd/browser.js"></script>
<script type="text/javascript" src="https://cdn.jsdelivr.net/gh/stdlib-js/random-streams-mt19937@umd/browser.js"></script>
<script type="text/javascript">
(function () {

function log( v ) {
    console.log( v.toString() );
}

var opts = {
    'objectMode': true,
    'iter': 10
};

var stream = randomStream( opts );

opts = {
    'objectMode': true
};
var iStream = inspectStream( opts, log );

stream.pipe( iStream );

})();
</script>
</body>
</html>
```

</section>

<!-- /.examples -->

<!-- Section for describing a command-line interface. -->



* * *

<section class="references">

## References

-   Matsumoto, Makoto, and Takuji Nishimura. 1998. "Mersenne Twister: A 623-dimensionally Equidistributed Uniform Pseudo-random Number Generator." _ACM Transactions on Modeling and Computer Simulation_ 8 (1). New York, NY, USA: ACM: 3–30. doi:[10.1145/272991.272995][@matsumoto:1998a].
-   Harase, Shin. 2017. "Conversion of Mersenne Twister to double-precision floating-point numbers." _ArXiv_ abs/1708.06018 (September). <https://arxiv.org/abs/1708.06018>.

</section>

<!-- /.references -->

<!-- Section for related `stdlib` packages. Do not manually edit this section, as it is automatically populated. -->

<section class="related">

* * *

## See Also

-   <span class="package-name">[`@stdlib/random-base/mt19937`][@stdlib/random/base/mt19937]</span><span class="delimiter">: </span><span class="description">A 32-bit Mersenne Twister pseudorandom number generator.</span>
-   <span class="package-name">[`@stdlib/random-iter/mt19937`][@stdlib/random/iter/mt19937]</span><span class="delimiter">: </span><span class="description">create an iterator for a 32-bit Mersenne Twister pseudorandom number generator.</span>
-   <span class="package-name">[`@stdlib/random-streams/minstd`][@stdlib/random/streams/minstd]</span><span class="delimiter">: </span><span class="description">create a readable stream for a linear congruential pseudorandom number generator (LCG) based on Park and Miller.</span>
-   <span class="package-name">[`@stdlib/random-streams/minstd-shuffle`][@stdlib/random/streams/minstd-shuffle]</span><span class="delimiter">: </span><span class="description">create a readable stream for a linear congruential pseudorandom number generator (LCG) whose output is shuffled.</span>
-   <span class="package-name">[`@stdlib/random-streams/randi`][@stdlib/random/streams/randi]</span><span class="delimiter">: </span><span class="description">create a readable stream for generating pseudorandom numbers having integer values.</span>
-   <span class="package-name">[`@stdlib/random-streams/randu`][@stdlib/random/streams/randu]</span><span class="delimiter">: </span><span class="description">create a readable stream for generating uniformly distributed pseudorandom numbers between 0 and 1.</span>

</section>

<!-- /.related -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->


<section class="main-repo" >

* * *

## Notice

This package is part of [stdlib][stdlib], a standard library for JavaScript and Node.js, with an emphasis on numerical and scientific computing. The library provides a collection of robust, high performance libraries for mathematics, statistics, streams, utilities, and more.

For more information on the project, filing bug reports and feature requests, and guidance on how to develop [stdlib][stdlib], see the main project [repository][stdlib].

#### Community

[![Chat][chat-image]][chat-url]

---

## License

See [LICENSE][stdlib-license].


## Copyright

Copyright &copy; 2016-2024. The Stdlib [Authors][stdlib-authors].

</section>

<!-- /.stdlib -->

<!-- Section for all links. Make sure to keep an empty line after the `section` element and another before the `/section` close. -->

<section class="links">

[npm-image]: http://img.shields.io/npm/v/@stdlib/random-streams-mt19937.svg
[npm-url]: https://npmjs.org/package/@stdlib/random-streams-mt19937

[test-image]: https://github.com/stdlib-js/random-streams-mt19937/actions/workflows/test.yml/badge.svg?branch=main
[test-url]: https://github.com/stdlib-js/random-streams-mt19937/actions/workflows/test.yml?query=branch:main

[coverage-image]: https://img.shields.io/codecov/c/github/stdlib-js/random-streams-mt19937/main.svg
[coverage-url]: https://codecov.io/github/stdlib-js/random-streams-mt19937?branch=main

<!--

[dependencies-image]: https://img.shields.io/david/stdlib-js/random-streams-mt19937.svg
[dependencies-url]: https://david-dm.org/stdlib-js/random-streams-mt19937/main

-->

[chat-image]: https://img.shields.io/gitter/room/stdlib-js/stdlib.svg
[chat-url]: https://app.gitter.im/#/room/#stdlib-js_stdlib:gitter.im

[stdlib]: https://github.com/stdlib-js/stdlib

[stdlib-authors]: https://github.com/stdlib-js/stdlib/graphs/contributors

[cli-section]: https://github.com/stdlib-js/random-streams-mt19937#cli
[cli-url]: https://github.com/stdlib-js/random-streams-mt19937/tree/cli
[@stdlib/random-streams-mt19937]: https://github.com/stdlib-js/random-streams-mt19937/tree/main

[umd]: https://github.com/umdjs/umd
[es-module]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules

[deno-url]: https://github.com/stdlib-js/random-streams-mt19937/tree/deno
[deno-readme]: https://github.com/stdlib-js/random-streams-mt19937/blob/deno/README.md
[umd-url]: https://github.com/stdlib-js/random-streams-mt19937/tree/umd
[umd-readme]: https://github.com/stdlib-js/random-streams-mt19937/blob/umd/README.md
[esm-url]: https://github.com/stdlib-js/random-streams-mt19937/tree/esm
[esm-readme]: https://github.com/stdlib-js/random-streams-mt19937/blob/esm/README.md
[branches-url]: https://github.com/stdlib-js/random-streams-mt19937/blob/main/branches.md

[stdlib-license]: https://raw.githubusercontent.com/stdlib-js/random-streams-mt19937/main/LICENSE

[stream]: https://nodejs.org/api/stream.html

[object-mode]: https://nodejs.org/api/stream.html#stream_object_mode

[readable-stream]: https://nodejs.org/api/stream.html

[@stdlib/array/uint32]: https://github.com/stdlib-js/array-uint32/tree/umd

[@matsumoto:1998a]: https://doi.org/10.1145/272991.272995

<!-- <related-links> -->

[@stdlib/random/base/mt19937]: https://github.com/stdlib-js/random-base-mt19937/tree/umd

[@stdlib/random/iter/mt19937]: https://github.com/stdlib-js/random-iter-mt19937/tree/umd

[@stdlib/random/streams/minstd]: https://github.com/stdlib-js/random-streams-minstd/tree/umd

[@stdlib/random/streams/minstd-shuffle]: https://github.com/stdlib-js/random-streams-minstd-shuffle/tree/umd

[@stdlib/random/streams/randi]: https://github.com/stdlib-js/random-streams-randi/tree/umd

[@stdlib/random/streams/randu]: https://github.com/stdlib-js/random-streams-randu/tree/umd

<!-- </related-links> -->

</section>

<!-- /.links -->
