# Primary Findings: `ebaenamar/wikidata-mcp` (Part 2)

This document continues the primary findings from the Context7 MCP server, focusing on the JavaScript and Node.js related code snippets. These snippets primarily originate from the `wikidata-mcp-npm/node_modules/` subdirectory, indicating a significant Node.js component within the `ebaenamar/wikidata-mcp` project.

---
**TITLE:** Express/Connect Top-Level Generic body-parser
**DESCRIPTION:** Demonstrates adding generic JSON and URL-encoded body parsers as top-level middleware in an Express application to parse all incoming requests. This is the simplest setup.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/body-parser/README.md#_snippet_15](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/body-parser/README.md#_snippet_15)
**LANGUAGE:** javascript
**CODE:**
```javascript
var express = require('express')
var bodyParser = require('body-parser')

var app = express()

// parse application/x-www-form-urlencoded
app.use(bodyParser.urlencoded({ extended: false }))

// parse application/json
app.use(bodyParser.json())

app.use(function (req, res) {
  res.setHeader('Content-Type', 'text/plain')
  res.write('you posted:\n')
  res.end(JSON.stringify(req.body, null, 2))
})
```
---
**TITLE:** Creating a Basic Express App (JavaScript)
**DESCRIPTION:** Demonstrates how to create a simple Express application, define a route for the root path, and start the server listening on port 3000.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/express/Readme.md#_snippet_0](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/express/Readme.md#_snippet_0)
**LANGUAGE:** JavaScript
**CODE:**
```javascript
const express = require('express')
const app = express()

app.get('/', function (req, res) {
  res.send('Hello World')
})

app.listen(3000)
```
---
**TITLE:** body-parser Custom Content Types
**DESCRIPTION:** Illustrates how to use the 'type' option with body-parser parsers to handle custom Content-Type headers, allowing parsing of non-standard JSON, raw data, or text.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/body-parser/README.md#_snippet_17](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/body-parser/README.md#_snippet_17)
**LANGUAGE:** javascript
**CODE:**
```javascript
var express = require('express')
var bodyParser = require('body-parser')

var app = express()

// parse various different custom JSON types as JSON
app.use(bodyParser.json({ type: 'application/*+json' }))

// parse some custom thing into a Buffer
app.use(bodyParser.raw({ type: 'application/vnd.custom-type' }))

// parse an HTML body into a string
app.use(bodyParser.text({ type: 'text/html' }))
```
---
**TITLE:** Converting String to Base64 using Vulnerable Buffer Constructor (Node.js)
**DESCRIPTION:** This JavaScript function demonstrates a security vulnerability by using the deprecated `new Buffer()` constructor with unchecked user input (`req.body.string`) to convert a string to Base64. If the input is a number instead of a string, it can lead to uninitialized memory exposure or excessive memory allocation, depending on the Node.js version.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/safer-buffer/Porting-Buffer.md#_snippet_6](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/safer-buffer/Porting-Buffer.md#_snippet_6)
**LANGUAGE:** javascript
**CODE:**
```javascript
function stringToBase64(req, res) {
  // The request body should have the format of `{ string: 'foobar' }`
  const rawBytes = new Buffer(req.body.string)
  const encoded = rawBytes.toString('base64')
  res.end({ encoded: encoded })
}
```
---
**TITLE:** Handling Middleware Error: request aborted (JavaScript)
**DESCRIPTION:** Happens when the client aborts the request before the body has been fully read. The error has status 400, type 'request.aborted', and includes the number of bytes received ('received') and expected ('expected').
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/body-parser/README.md#_snippet_11](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/body-parser/README.md#_snippet_11)
---
**TITLE:** Handling User Name with Cookies in Node.js HTTP Server
**DESCRIPTION:** This Node.js example creates an HTTP server that parses incoming requests, checks for a 'name' query parameter to set a cookie using cookie.serialize with httpOnly and maxAge, or parses existing cookies using cookie.parse to retrieve a stored name and display a personalized message. It uses url, http, cookie, and escape-html modules.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/cookie/README.md#_snippet_4](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/cookie/README.md#_snippet_4)
**LANGUAGE:** javascript
**CODE:**
```javascript
var cookie = require('cookie');
var escapeHtml = require('escape-html');
var http = require('http');
var url = require('url');

function onRequest(req, res) {
  // Parse the query string
  var query = url.parse(req.url, true, true).query;

  if (query && query.name) {
    // Set a new cookie with the name
    res.setHeader('Set-Cookie', cookie.serialize('name', String(query.name), {
      httpOnly: true,
      maxAge: 60 * 60 * 24 * 7 // 1 week
    }));

    // Redirect back after setting cookie
    res.statusCode = 302;
    res.setHeader('Location', req.headers.referer || '/');
    res.end();
    return;
  }

  // Parse the cookies on the request
  var cookies = cookie.parse(req.headers.cookie || '');

  // Get the visitor name set in the cookie
  var name = cookies.name;

  res.setHeader('Content-Type', 'text/html; charset=UTF-8');

  if (name) {
    res.write('<p>Welcome back, <b>' + escapeHtml(name) + '</b>!</p>');
  } else {
    res.write('<p>Hello, new visitor!</p>');
  }

  res.write('<form method="GET">');
  res.write('<input placeholder="enter your name" name="name"> <input type="submit" value="Set Name">');
  res.end('</form>');
}

http.createServer(onRequest).listen(3000);
```
---
**TITLE:** Express Route-Specific body-parser
**DESCRIPTION:** Shows how to apply body parsers specifically to individual routes in an Express application, which is generally the recommended approach for using body-parser.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/body-parser/README.md#_snippet_16](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/body-parser/README.md#_snippet_16)
**LANGUAGE:** javascript
**CODE:**
```javascript
var express = require('express')
var bodyParser = require('body-parser')

var app = express()

// create application/json parser
var jsonParser = bodyParser.json()

// create application/x-www-form-urlencoded parser
var urlencodedParser = bodyParser.urlencoded({ extended: false })

// POST /login gets urlencoded bodies
app.post('/login', urlencodedParser, function (req, res) {
  res.send('welcome, ' + req.body.username)
})

// POST /api/users gets JSON bodies
app.post('/api/users', jsonParser, function (req, res) {
  // create user in req.body
})
```
---
**TITLE:** Serving Static Files with Node.js HTTP Server (JavaScript)
**DESCRIPTION:** Demonstrates how to use serve-static with Node.js's built-in http module and finalhandler to create a basic web server that serves static files from a specified directory.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/serve-static/README.md#_snippet_2](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/serve-static/README.md#_snippet_2)
**LANGUAGE:** js
**CODE:**
```javascript
var finalhandler = require('finalhandler')
var http = require('http')
var serveStatic = require('serve-static')

// Serve up public/ftp folder
var serve = serveStatic('public/ftp', { index: ['index.html', 'index.htm'] })

// Create server
var server = http.createServer(function onRequest (req, res) {
  serve(req, res, finalhandler(req, res))
})

// Listen
server.listen(3000)
```
---
**TITLE:** Installing Express via npm (Console)
**DESCRIPTION:** Command to install the Express web framework package locally within a Node.js project using the npm package manager.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/express/Readme.md#_snippet_1](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/express/Readme.md#_snippet_1)
**LANGUAGE:** Console
**CODE:**
```console
$ npm install express
```
---
**TITLE:** Processing HTTP Request Body with Promises (TypeScript)
**DESCRIPTION:** Shows how to integrate the `raw-body` library with Promises in a TypeScript Node.js HTTP server. It reads the request body and sends back its size or an error, using TypeScript import syntax. Requires the `raw-body` and `http` modules.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/raw-body/README.md#_snippet_6](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/raw-body/README.md#_snippet_6)
**LANGUAGE:** ts
**CODE:**
```typescript
import * as getRawBody from 'raw-body';
import * as http from 'http';

const server = http.createServer((req, res) => {
  getRawBody(req)
  .then((buf) => {
    res.statusCode = 200;
    res.end(buf.length + ' bytes submitted');
  })
  .catch((err) => {
    res.statusCode = err.statusCode;
    res.end(err.message);
  });
});

server.listen(3000);
```
---
**TITLE:** Using fresh in a Node.js HTTP Server
**DESCRIPTION:** Illustrates integrating the 'fresh' module into a Node.js HTTP server to handle conditional requests (If-None-Match, If-Modified-Since) and respond with 304 Not Modified when the client's cache is fresh.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/fresh/README.md#_snippet_3](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/fresh/README.md#_snippet_3)
**LANGUAGE:** javascript
**CODE:**
```javascript
var fresh = require('fresh')
var http = require('http')

var server = http.createServer(function (req, res) {
  // perform server logic
  // ... including adding ETag / Last-Modified response headers

  if (isFresh(req, res)) {
    // client has a fresh copy of resource
    res.statusCode = 304
    res.end()
    return
  }

  // send the resource
  res.statusCode = 200
  res.end('hello, world!')
})

function isFresh (req, res) {
  return fresh(req.headers, {
    'etag': res.getHeader('ETag'),
    'last-modified': res.getHeader('Last-Modified')
  })
}

server.listen(3000)
```
---
**TITLE:** Simple static file serving with Express and serve-static
**DESCRIPTION:** Shows a basic setup for serving static files using the Express framework and the `serve-static` middleware. It configures `serve-static` to serve files from the 'public/ftp' directory, specifying 'default.html' and 'default.htm' as potential index files. Requires `express` and `serve-static`.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/serve-static/README.md#_snippet_4](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/serve-static/README.md#_snippet_4)
**LANGUAGE:** javascript
**CODE:**
```javascript
var express = require('express')
var serveStatic = require('serve-static')

var app = express()

app.use(serveStatic('public/ftp', { index: ['default.html', 'default.htm'] }))
app.listen(3000)
```
---
**TITLE:** Using proxyaddr with IP/CIDR/Netmask Trust
**DESCRIPTION:** These examples demonstrate using the `proxyaddr` function with trust defined as a single IP string or an array of IP addresses, CIDR strings, or IP/netmask strings for both IPv4 and IPv6.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/proxy-addr/README.md#_snippet_3](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/proxy-addr/README.md#_snippet_3)
**LANGUAGE:** JavaScript
**CODE:**
```javascript
proxyaddr(req, '127.0.0.1')
```
**LANGUAGE:** JavaScript
**CODE:**
```javascript
proxyaddr(req, ['127.0.0.0/8', '10.0.0.0/8'])
```
**LANGUAGE:** JavaScript
**CODE:**
```javascript
proxyaddr(req, ['127.0.0.0/255.0.0.0', '192.168.0.0/255.255.0.0'])
```
**LANGUAGE:** JavaScript
**CODE:**
```javascript
proxyaddr(req, '::1')
```
**LANGUAGE:** JavaScript
**CODE:**
```javascript
proxyaddr(req, ['::1/128', 'fe80::/10'])
```
---
**TITLE:** Applying Function.prototype.bind Polyfill in JavaScript
**DESCRIPTION:** This snippet demonstrates how to apply the 'function-bind' polyfill to the global Function.prototype.bind, making it available in environments that do not natively support it. This is typically done at the entry point of an application to ensure compatibility.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/function-bind/README.md#_snippet_0](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/function-bind/README.md#_snippet_0)
**LANGUAGE:** JavaScript
**CODE:**
```javascript
Function.prototype.bind = require("function-bind")
```
---
**TITLE:** Using proxyaddr with Predefined Name Trust
**DESCRIPTION:** These examples illustrate using the `proxyaddr` function with predefined names like 'loopback', 'linklocal', or 'uniquelocal' to specify trusted addresses, or combining them with specific addresses/CIDRs.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/proxy-addr/README.md#_snippet_4](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/proxy-addr/README.md#_snippet_4)
**LANGUAGE:** JavaScript
**CODE:**
```javascript
proxyaddr(req, 'loopback')
```
**LANGUAGE:** JavaScript
**CODE:**
```javascript
proxyaddr(req, ['loopback', 'fc00:ac:1ab5:fff::1/64'])
```
---
**TITLE:** Configuring Proxy Trust with app.set('trust proxy') (JavaScript)
**DESCRIPTION:** Details the `app.set('trust proxy', trust)` option for configuring how the application trusts proxy headers (like X-Forwarded-For), supporting numbers (hops), 'loopback', single IPs, and CIDR subnets.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/express/History.md#_snippet_10](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/express/History.md#_snippet_10)
**LANGUAGE:** JavaScript
**CODE:**
```javascript
// Trust the first hop
app.set('trust proxy', 1);

// Trust loopback addresses
app.set('trust proxy', 'loopback');

// Trust a single IP address
app.set('trust proxy', '10.0.0.1');

// Trust a subnet
app.set('trust proxy', '10.0.0.1/16');
```
---
**TITLE:** Converting Time Strings to Milliseconds (JavaScript)
**DESCRIPTION:** Demonstrates how to use the `ms` function to convert various time format strings (like '2 days', '10h', '5s', '1y') into their equivalent value in milliseconds. Also shows handling pure number strings.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/ms/readme.md#_snippet_0](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/ms/readme.md#_snippet_0)
**LANGUAGE:** JavaScript
**CODE:**
```javascript
ms('2 days')  // 172800000
ms('1d')      // 86400000
ms('10h')     // 36000000
ms('2.5 hrs') // 9000000
ms('2h')      // 7200000
ms('1m')      // 60000
ms('5s')      // 5000
ms('1y')      // 31557600000
ms('100')     // 100
```
---
**TITLE:** Creating Buffer from non-number (Node.js Compatibility)
**DESCRIPTION:** Provides a compatibility solution for creating a Buffer from a non-number input. It uses Buffer.from if available or falls back to new Buffer with a type check to prevent security issues, as recommended due to the deprecation reasons.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/safer-buffer/Porting-Buffer.md#_snippet_3](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/safer-buffer/Porting-Buffer.md#_snippet_3)
**LANGUAGE:** JavaScript
**CODE:**
```javascript
var buf;
if (Buffer.from && Buffer.from !== Uint8Array.from) {
  buf = Buffer.from(notNumber, encoding);
} else {
  if (typeof notNumber === 'number')
    throw new Error('The "size" argument must be of type number.');
  buf = new Buffer(notNumber, encoding);
}
```
---
**TITLE:** Parsing Nested Objects with qs
**DESCRIPTION:** Demonstrates how `qs.parse` automatically creates nested JavaScript objects when the query string uses square brackets (`[]`) around sub-keys.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/qs/README.md#_snippet_2](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/qs/README.md#_snippet_2)
**LANGUAGE:** javascript
**CODE:**
```javascript
assert.deepEqual(qs.parse('foo[bar]=baz'), {
    foo: {
        bar: 'baz'
    }
});
```
---
**TITLE:** Define a lazy property in JavaScript
**DESCRIPTION:** Demonstrates how to use defineLazyProperty to add a lazily evaluated 'rainbow' property to an object. The value is computed only when the property is first accessed, typically upon a user action or other event.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/define-lazy-prop/readme.md#_snippet_1](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/define-lazy-prop/readme.md#_snippet_1)
**LANGUAGE:** JavaScript
**CODE:**
```javascript
import defineLazyProperty from 'define-lazy-prop';

const unicorn = {
	// …
};

defineLazyProperty(unicorn, 'rainbow', () => expensiveComputation());

app.on('user-action', () => {
	doSomething(unicorn.rainbow);
});
```
---
**TITLE:** Pipe SendStream to HTTP Response
**DESCRIPTION:** Create a new SendStream instance for a given request and path, then pipe it to the Node.js HTTP response object to send the file.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/send/README.md#_snippet_2](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/send/README.md#_snippet_2)
**LANGUAGE:** js
**CODE:**
```javascript
send(req, path, options).pipe(res)
```
---
**TITLE:** Use raw-body in an Express application
**DESCRIPTION:** An example middleware function for an Express application that uses `raw-body` to parse the request body, handling content length, size limits, and character encoding. It stores the parsed body as `req.text`. Requires `content-type` and `express`.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/raw-body/README.md#_snippet_3](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/raw-body/README.md#_snippet_3)
**LANGUAGE:** js
**CODE:**
```javascript
var contentType = require('content-type')
var express = require('express')
var getRawBody = require('raw-body')

var app = express()

app.use(function (req, res, next) {
  getRawBody(req, {
    length: req.headers['content-length'],
    limit: '1mb',
    encoding: contentType.parse(req).parameters.charset
  }, function (err, string) {
    if (err) return next(err)
    req.text = string
    next()
  })
})

// now access req.text
```
---
**TITLE:** Parsing an HTTP Cookie header string
**DESCRIPTION:** Demonstrates how to use the `cookie.parse` function to parse a raw 'Cookie' header string into a JavaScript object. It handles URL-decoded values by default.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/cookie/README.md#_snippet_2](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/cookie/README.md#_snippet_2)
**LANGUAGE:** js
**CODE:**
```javascript
var cookies = cookie.parse('foo=bar; equation=E%3Dmc%5E2');
// { foo: 'bar', equation: 'E=mc^2' }
```
---
**TITLE:** Perform Accept-Language Header Negotiation in JavaScript
**DESCRIPTION:** Demonstrates how to use the Negotiator class to determine the most preferred language(s) based on the Accept-Language header and a list of available languages.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/negotiator/README.md#_snippet_3](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/negotiator/README.md#_snippet_3)
**LANGUAGE:** JavaScript
**CODE:**
```javascript
negotiator = new Negotiator(request)

availableLanguages = ['en', 'es', 'fr']

// Let's say Accept-Language header is 'en;q=0.8, es, pt'

negotiator.languages()
// -> ['es', 'pt', 'en']

negotiator.languages(availableLanguages)
// -> ['es', 'en']

language = negotiator.language(availableLanguages)
// -> 'es'
```
---
**TITLE:** Importing the on-finished module (JavaScript)
**DESCRIPTION:** Imports the `on-finished` module into a JavaScript file using the `require` function. This makes the `onFinished` function available for use.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/on-finished/README.md#_snippet_1](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/on-finished/README.md#_snippet_1)
**LANGUAGE:** JavaScript
**CODE:**
```javascript
var onFinished = require('on-finished')
```
---
**TITLE:** Parsing Deeply Nested Objects with qs
**DESCRIPTION:** Illustrates how `qs.parse` handles multiple levels of nesting in the query string using chained square brackets, creating a corresponding deep object structure.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/qs/README.md#_snippet_6](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/qs/README.md#_snippet_6)
**LANGUAGE:** javascript
**CODE:**
```javascript
assert.deepEqual(qs.parse('foo[bar][baz]=foobarbaz'), {
    foo: {
        bar: {
            baz: 'foobarbaz'
        }
    }
});
```
---
**TITLE:** Creating a Basic HTTP Error
**DESCRIPTION:** Shows the simplest way to create a new HTTP error object with a specific status code and message using the createError function.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/http-errors/README.md#_snippet_2](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/http-errors/README.md#_snippet_2)
**LANGUAGE:** javascript
**CODE:**
```javascript
var err = createError(404, 'This video does not exist!')
```
---
**TITLE:** Requiring the ee-first Module in Node.js
**DESCRIPTION:** Imports the ee-first module into a JavaScript variable named `first`. This is the standard way to use the module in Node.js environments.
**SOURCE:** [https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/ee-first/README.md#_snippet_1](https://github.com/ebaenamar/wikidata-mcp/blob/master/wikidata-mcp-npm/node_modules/ee-first/README.md#_snippet_1)
**LANGUAGE:** js
**CODE:**
```javascript
var first = require('ee-first')
```
---
This file contains the first set of JavaScript/Node.js related snippets. More will follow in subsequent parts.