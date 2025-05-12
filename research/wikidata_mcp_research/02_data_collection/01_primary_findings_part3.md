# Primary Findings: `ebaenamar/wikidata-mcp` (Part 3)

This document continues the primary findings from the Context7 MCP server, focusing on the remaining JavaScript and Node.js related code snippets. These snippets primarily originate from the `wikidata-mcp-npm/node_modules/` subdirectory.

---
**TITLE:** Basic Usage Examples for Open Package
**DESCRIPTION:** Demonstrates various ways to use the 'open' function, including opening files, URLs, specifying browsers, using arguments, and opening apps directly with 'openApp'.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/open/readme.md#_snippet_1](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/open/readme.md#_snippet_1)
**LANGUAGE:** js
**CODE:**
```javascript
import open, {openApp, apps} from 'open';

// Opens the image in the default image viewer and waits for the opened app to quit.
await open('unicorn.png', {wait: true});
console.log('The image viewer app quit');

// Opens the URL in the default browser.
await open('https://sindresorhus.com');

// Opens the URL in a specified browser.
await open('https://sindresorhus.com', {app: {name: 'firefox'}});

// Specify app arguments.
await open('https://sindresorhus.com', {app: {name: 'google chrome', arguments: ['--incognito']}});

// Opens the URL in the default browser in incognito mode.
await open('https://sindresorhus.com', {app: {name: apps.browserPrivate}});

// Open an app.
await openApp('xcode');

// Open an app with arguments.
await openApp(apps.chrome, {arguments: ['--incognito']});
```
---
**TITLE:** Installing get-proto package
**DESCRIPTION:** Command to install the 'get-proto' package using npm.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/get-proto/README.md#_snippet_0](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/get-proto/README.md#_snippet_0)
**LANGUAGE:** shell
**CODE:**
```shell
npm install --save get-proto
```
---
**TITLE:** Installing object-inspect via npm (Shell)
**DESCRIPTION:** Provides the command-line instruction to install the `object-inspect` package using the npm package manager. This is the standard way to add the library as a dependency to a Node.js project.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/object-inspect/readme.markdown#_snippet_3](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/object-inspect/readme.markdown#_snippet_3)
**LANGUAGE:** Shell
**CODE:**
```shell
npm install object-inspect
```
---
**TITLE:** Installing content-disposition Module
**DESCRIPTION:** This command demonstrates how to install the 'content-disposition' module using the npm package manager.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/content-disposition/README.md#_snippet_0](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/content-disposition/README.md#_snippet_0)
**LANGUAGE:** Shell
**CODE:**
```shell
$ npm install content-disposition
```
---
**TITLE:** Installing encodeurl (Shell)
**DESCRIPTION:** This command demonstrates how to install the `encodeurl` module using the npm package manager. It adds the module to your project's dependencies.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/send/node_modules/encodeurl/README.md#_snippet_0](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/send/node_modules/encodeurl/README.md#_snippet_0)
**LANGUAGE:** sh
**CODE:**
```sh
npm install encodeurl
```
---
**TITLE:** Installing array-flatten using npm
**DESCRIPTION:** Installs the 'array-flatten' package from the npm registry and saves it as a production dependency in the project's package.json file.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/array-flatten/README.md#_snippet_0](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/array-flatten/README.md#_snippet_0)
**LANGUAGE:** Shell
**CODE:**
```shell
npm install array-flatten --save
```
---
**TITLE:** Creating Buffer from non-number (Deprecated Node.js)
**DESCRIPTION:** Shows the old, deprecated way of creating a Buffer from a non-number input using the Buffer constructor. This method is problematic and can lead to security vulnerabilities.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/safer-buffer/Porting-Buffer.md#_snippet_2](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/safer-buffer/Porting-Buffer.md#_snippet_2)
**LANGUAGE:** JavaScript
**CODE:**
```javascript
var buf = new Buffer(notNumber, encoding);
```
---
**TITLE:** Install content-type module via npm
**DESCRIPTION:** Command to install the 'content-type' package from the npm registry using the npm command-line interface.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/content-type/README.md#_snippet_0](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/content-type/README.md#_snippet_0)
**LANGUAGE:** sh
**CODE:**
```sh
$ npm install content-type
```
---
**TITLE:** Installing the 'vary' module using npm
**DESCRIPTION:** Command to install the 'vary' package from the npm registry for use in a Node.js project.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/vary/README.md#_snippet_0](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/vary/README.md#_snippet_0)
**LANGUAGE:** Shell
**CODE:**
```shell
npm install vary
```
---
**TITLE:** Installing etag Node.js Module
**DESCRIPTION:** Install the 'etag' module using npm. This command adds the package to your project's dependencies.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/etag/README.md#_snippet_0](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/etag/README.md#_snippet_0)
**LANGUAGE:** sh
**CODE:**
```sh
npm install etag
```
---
**TITLE:** Example: Closing a stream on Response finish (JavaScript)
**DESCRIPTION:** Demonstrates how to use `on-finished` with an HTTP server. It creates a read stream, pipes it to the response, and uses `on-finished` to ensure the stream is destroyed (closed) once the response has completed, preventing resource leaks.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/on-finished/README.md#_snippet_4](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/on-finished/README.md#_snippet_4)
**LANGUAGE:** JavaScript
**CODE:**
```javascript
var destroy = require('destroy')
var fs = require('fs')
var http = require('http')
var onFinished = require('on-finished')

http.createServer(function onRequest (req, res) {
  var stream = fs.createReadStream('package.json')
  stream.pipe(res)
  onFinished(res, function () {
    destroy(stream)
  })
})
```
---
**TITLE:** Custom static file caching settings with Express and serve-static
**DESCRIPTION:** Provides an example of setting different caching headers based on file type when serving static files with Express and `serve-static`. It uses the `setHeaders` option to apply a custom function that sets `Cache-Control` to `max-age=0` for HTML files while applying a default `maxAge` of '1d' for others. Requires `express`, `path`, and `serve-static`.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/serve-static/README.md#_snippet_6](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/serve-static/README.md#_snippet_6)
**LANGUAGE:** javascript
**CODE:**
```javascript
var express = require('express')
var path = require('path')
var serveStatic = require('serve-static')

var app = express()

app.use(serveStatic(path.join(__dirname, 'public'), {
  maxAge: '1d',
  setHeaders: setCustomCacheControl
}))

app.listen(3000)

function setCustomCacheControl (res, path) {
  if (serveStatic.mime.lookup(path) === 'text/html') {
    // Custom Cache-Control for HTML files
    res.setHeader('Cache-Control', 'public, max-age=0')
  }
}
```
---
**TITLE:** Serving static files from multiple directories with Express
**DESCRIPTION:** Illustrates how to configure Express to serve static files by searching through multiple directories in a specific order. It uses `serve-static` twice with different paths ('public-optimized' and 'public'), allowing 'public-optimized' to be checked first as the primary source, falling back to 'public'. Requires `express`, `path`, and `serve-static`.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/serve-static/README.md#_snippet_5](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/serve-static/README.md#_snippet_5)
**LANGUAGE:** javascript
**CODE:**
```javascript
var express = require('express')
var path = require('path')
var serveStatic = require('serve-static')

var app = express()

app.use(serveStatic(path.join(__dirname, 'public-optimized')))
app.use(serveStatic(path.join(__dirname, 'public')))
app.listen(3000)
```
---
**TITLE:** Perform Accept-Charset Header Negotiation in JavaScript
**DESCRIPTION:** Demonstrates how to use the Negotiator class to determine the most preferred charset(s) based on the Accept-Charset header and a list of available charsets.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/negotiator/README.md#_snippet_4](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/negotiator/README.md#_snippet_4)
**LANGUAGE:** JavaScript
**CODE:**
```javascript
availableCharsets = ['utf-8', 'iso-8859-1', 'iso-8859-5']

negotiator = new Negotiator(request)

// Let's say Accept-Charset header is 'utf-8, iso-8859-1;q=0.8, utf-7;q=0.2'

negotiator.charsets()
// -> ['utf-8', 'iso-8859-1', 'utf-7']

negotiator.charsets(availableCharsets)
// -> ['utf-8', 'iso-8859-1']

negotiator.charset(availableCharsets)
// -> 'utf-8'
```
---
**TITLE:** Simple HTTP Type Negotiation with accepts
**DESCRIPTION:** Demonstrates how to use the 'accepts' module within an HTTP server to negotiate content types (JSON, HTML, or plain text) based on the client's 'Accept' header.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/accepts/README.md#_snippet_1](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/accepts/README.md#_snippet_1)
**LANGUAGE:** js
**CODE:**
```javascript
var accepts = require('accepts')\nvar http = require('http')\n\nfunction app (req, res) {\n  var accept = accepts(req)\n\n  // the order of this list is significant; should be server preferred order\n  switch (accept.type(['json', 'html'])) {\n    case 'json':\n      res.setHeader('Content-Type', 'application/json')\n      res.write('{\"hello\":\"world!\"}')\n      break\n    case 'html':\n      res.setHeader('Content-Type', 'text/html')\n      res.write('<b>hello, world!</b>')\n      break\n    default:\n      // the fallback is text/plain, so no need to specify it above\n      res.setHeader('Content-Type', 'text/plain')\n      res.write('hello, world!')\n      break\n  }\n\n  res.end()\n}\n\nhttp.createServer(app).listen(3000)
```
---
**TITLE:** Perform Accept Header Negotiation in JavaScript
**DESCRIPTION:** Demonstrates how to use the Negotiator class to determine the most preferred media type(s) based on the Accept header and a list of available types.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/negotiator/README.md#_snippet_2](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/negotiator/README.md#_snippet_2)
**LANGUAGE:** JavaScript
**CODE:**
```javascript
availableMediaTypes = ['text/html', 'text/plain', 'application/json']

// The negotiator constructor receives a request object
negotiator = new Negotiator(request)

// Let's say Accept header is 'text/html, application/*;q=0.2, image/jpeg;q=0.8'

negotiator.mediaTypes()
// -> ['text/html', 'image/jpeg', 'application/*']

negotiator.mediaTypes(availableMediaTypes)
// -> ['text/html', 'application/json']

negotiator.mediaType(availableMediaTypes)
// -> 'text/html'
```
---
**TITLE:** Using iconv-lite Streaming API in Node.js
**DESCRIPTION:** Illustrates how to use iconv-lite with Node.js streams for encoding and decoding data chunk-by-chunk. Examples include decoding an incoming HTTP request stream, piping streams for file encoding conversion, and using the `.collect()` method to accumulate stream data.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/iconv-lite/README.md#_snippet_1](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/iconv-lite/README.md#_snippet_1)
**LANGUAGE:** javascript
**CODE:**
```javascript

// Decode stream (from binary stream to js strings)
http.createServer(function(req, res) {
    var converterStream = iconv.decodeStream('win1251');
    req.pipe(converterStream);

    converterStream.on('data', function(str) {
        console.log(str); // Do something with decoded strings, chunk-by-chunk.
    });
});

// Convert encoding streaming example
fs.createReadStream('file-in-win1251.txt')
    .pipe(iconv.decodeStream('win1251'))
    .pipe(iconv.encodeStream('ucs2'))
    .pipe(fs.createWriteStream('file-in-ucs2.txt'));

// Sugar: all encode/decode streams have .collect(cb) method to accumulate data.
http.createServer(function(req, res) {
    req.pipe(iconv.decodeStream('win1251')).collect(function(err, body) {
        assert(typeof body == 'string');
        console.log(body); // full request body string
    });
});
```
---
**TITLE:** Install mime package - npm
**DESCRIPTION:** Install the mime package using the npm package manager.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/mime/README.md#_snippet_0](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/mime/README.md#_snippet_0)
**LANGUAGE:** bash
**CODE:**
```bash
npm install mime
```
---
**TITLE:** Buffer.from(str, encoding) Example
**DESCRIPTION:** Creates a new Buffer containing the bytes of a given string, using a specified character encoding (defaults to 'utf8'). Throws a TypeError if the input is not a string.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/safe-buffer/README.md#_snippet_6](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/safe-buffer/README.md#_snippet_6)
**LANGUAGE:** JavaScript
**CODE:**
```javascript
const buf1 = Buffer.from('this is a tést');
console.log(buf1.toString());
  // prints: this is a tést
console.log(buf1.toString('ascii'));
  // prints: this is a tC)st

const buf2 = Buffer.from('7468697320697320612074c3a97374', 'hex');
console.log(buf2.toString());
  // prints: this is a tést
```
---
**TITLE:** Serving files as downloads with Node.js http and serve-static
**DESCRIPTION:** Demonstrates how to create a basic HTTP server using Node.js's built-in `http` module and the `serve-static` middleware. It configures `serve-static` to serve files from the 'public/ftp' directory and uses a custom `setHeaders` function to force all served files to be downloaded by setting the `Content-Disposition` header. Requires `content-disposition`, `finalhandler`, `http`, and `serve-static`.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/serve-static/README.md#_snippet_3](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/serve-static/README.md#_snippet_3)
**LANGUAGE:** javascript
**CODE:**
```javascript
var contentDisposition = require('content-disposition')
var finalhandler = require('finalhandler')
var http = require('http')
var serveStatic = require('serve-static')

// Serve up public/ftp folder
var serve = serveStatic('public/ftp', {
  index: false,
  setHeaders: setHeaders
})

// Set header to force download
function setHeaders (res, path) {
  res.setHeader('Content-Disposition', contentDisposition(path))
}

// Create server
var server = http.createServer(function onRequest (req, res) {
  serve(req, res, finalhandler(req, res))
})

// Listen
server.listen(3000)
```
---
**TITLE:** Configuring Session Middleware (JavaScript)
**DESCRIPTION:** Shows the updated syntax for configuring the session middleware using the `use()` method with an options object, replacing the deprecated `set("session cookie")` method.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/express/History.md#_snippet_109](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/express/History.md#_snippet_109)
**LANGUAGE:** JavaScript
**CODE:**
```javascript
use(Session, { cookie: { ... }})
```
---
**TITLE:** Basic Parsing and Stringifying with qs in JavaScript
**DESCRIPTION:** Demonstrates the basic usage of the qs library to parse a query string into an object and stringify an object back into a query string. It uses Node.js require and assert modules for demonstration.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/qs/README.md#_snippet_0](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/qs/README.md#_snippet_0)
**LANGUAGE:** javascript
**CODE:**
```javascript
var qs = require('qs');
var assert = require('assert');

var obj = qs.parse('a=c');
assert.deepEqual(obj, { a: 'c' });

var str = qs.stringify(obj);
assert.equal(str, 'a=c');
```
---
**TITLE:** Parsing HTTP Range Header - JavaScript
**DESCRIPTION:** Demonstrates how to use the 'parseRange' function to parse an HTTP 'Range' header string. It shows how to check the range type and iterate over the parsed ranges. The function returns an array of ranges or a negative error code (-1 for unsatisfiable, -2 for malformed).
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/range-parser/README.md#_snippet_2](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/range-parser/README.md#_snippet_2)
**LANGUAGE:** JavaScript
**CODE:**
```javascript
// parse header from request
var range = parseRange(size, req.headers.range)

// the type of the range
if (range.type === 'bytes') {
  // the ranges
  range.forEach(function (r) {
    // do something with r.start and r.end
  })
}
```
---
**TITLE:** Example HTTP Server using vary() for User-Agent sniffing
**DESCRIPTION:** A complete Node.js HTTP server example demonstrating how to use `vary(res, 'User-Agent')` to indicate that the response content depends on the `User-Agent` request header. It sniffs the user agent and serves different content based on whether it's mobile or not.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/vary/README.md#_snippet_4](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/vary/README.md#_snippet_4)
**LANGUAGE:** JavaScript
**CODE:**
```javascript
var http = require('http')
var vary = require('vary')

http.createServer(function onRequest (req, res) {
  // about to user-agent sniff
  vary(res, 'User-Agent')

  var ua = req.headers['user-agent'] || ''
  var isMobile = /mobi|android|touch|mini/i.test(ua)

  // serve site, depending on isMobile
  res.setHeader('Content-Type', 'text/html')
  res.end('You are (probably) ' + (isMobile ? '' : 'not ') + 'a mobile user')
})
```
---
**TITLE:** Parsing Query Strings with Duplicate Keys (qs, JavaScript)
**DESCRIPTION:** Shows the default behavior of `qs.parse` for duplicate keys (combining into an array) and how to control this behavior using the `duplicates` option with values 'combine', 'first', and 'last'. Uses `assert.deepEqual` to verify the output.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/qs/README.md#_snippet_17](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/qs/README.md#_snippet_17)
**LANGUAGE:** JavaScript
**CODE:**
```javascript
assert.deepEqual(qs.parse('foo=bar&foo=baz'), { foo: ['bar', 'baz'] });
assert.deepEqual(qs.parse('foo=bar&foo=baz', { duplicates: 'combine' }), { foo: ['bar', 'baz'] });
assert.deepEqual(qs.parse('foo=bar&foo=baz', { duplicates: 'first' }), { foo: 'bar' });
assert.deepEqual(qs.parse('foo=bar&foo=baz', { duplicates: 'last' }), { foo: 'baz' });
```
---
**TITLE:** Extending an Existing Error with http-errors
**DESCRIPTION:** Illustrates how to wrap a standard Node.js error (like from fs.readFile) with http-errors properties, allowing it to be treated as an HTTP error while preserving original error details.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/http-errors/README.md#_snippet_3](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/http-errors/README.md#_snippet_3)
**LANGUAGE:** javascript
**CODE:**
```javascript
fs.readFile('foo.txt', function (err, buf) {
  if (err) {
    if (err.code === 'ENOENT') {
      var httpError = createError(404, err, { expose: false })
    } else {
      var httpError = createError(500, err)
    }
  }
})
```
---
**TITLE:** Parse Content-Type string using content-type.parse
**DESCRIPTION:** Parses a raw Content-Type header string into an object containing the media 'type' and 'parameters'. The 'type' and parameter names are lower-cased. Throws a TypeError if the input string is missing or invalid.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/content-type/README.md#_snippet_2](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/content-type/README.md#_snippet_2)
**LANGUAGE:** js
**CODE:**
```javascript
var obj = contentType.parse('image/svg+xml; charset=utf-8')
```
---
**TITLE:** Using http-errors in Express Middleware
**DESCRIPTION:** Demonstrates how to integrate http-errors into an Express.js application middleware to handle authentication checks and generate HTTP errors.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/http-errors/README.md#_snippet_1](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/http-errors/README.md#_snippet_1)
**LANGUAGE:** javascript
**CODE:**
```javascript
var createError = require('http-errors')
var express = require('express')
var app = express()

app.use(function (req, res, next) {
  if (!req.user) return next(createError(401, 'Please login to view this page.'))
  next()
})
```
---
**TITLE:** Sending File with Content-Disposition Header (Node.js)
**DESCRIPTION:** This comprehensive example illustrates how to create a simple HTTP server that sends a file, setting the Content-Type and Content-Disposition headers correctly for file download.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/content-disposition/README.md#_snippet_4](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/content-disposition/README.md#_snippet_4)
**LANGUAGE:** JavaScript
**CODE:**
```javascript
var contentDisposition = require('content-disposition')
var destroy = require('destroy')
var fs = require('fs')
var http = require('http')
var onFinished = require('on-finished')

var filePath = '/path/to/public/plans.pdf'

http.createServer(function onRequest (req, res) {
  // set headers
  res.setHeader('Content-Type', 'application/pdf')
  res.setHeader('Content-Disposition', contentDisposition(filePath))

  // send file
  var stream = fs.createReadStream(filePath)
  stream.pipe(res)
  onFinished(res, function () {
    destroy(stream)
  })
})
```
---
This concludes the primary findings from the initial Context7 `get-library-docs` query.