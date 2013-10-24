# RFC 6455 WebSockets support for Racket

This package provides an
[RFC 6455](http://tools.ietf.org/html/rfc6455) compatible WebSockets
*server* interface for Racket, building on Racket's `web-server`
collection. Racket's built-in `net/websocket` collection implements an
old draft of the WebSockets protocol that is no longer supported by
modern browsers, whereas RFC 6455 is the standard. Wikipedia has a
[good section on browser support for WebSocket protocol variations](http://en.wikipedia.org/wiki/WebSocket#Browser_support),
which states "All the latest browsers except Android browser support
the latest specification (RFC 6455) of the WebSocket protocol."

In particular, this package has been tested against

 - Firefox 24.0
 - Chrome 30.0.1599.101

It does not work in Safari 5.x; only Safari 6.0 and newer have support
for RFC 6455 WebSockets. In principle, it ought to be possible to
combine this package's implementation with a fallback to the older
protocol variant implemented by `net/websocket/server`, but I have not
explored this.

## Usage Synopsis

Using the `net/websocket`-compatible interface:

```racket
(require net/rfc6455)
(ws-serve #:port 8081 (lambda (c s) (ws-send! c "Hello world!")))
```

Using an interface that supports URL path matching and WebSocket
subprotocol selection:

```racket
(require net/rfc6455)
(ws-serve* #:port 8081
           (ws-service-mapper
		    ["/test" ;; the URL path (regular expression)
			 [(subprotocol) ;; if client requests subprotocol "subprotocol"
			  (lambda (c) (ws-send! c "You requested a subprotocol"))]
		     [(#f)          ;; if client did not request any subprotocol
			  (lambda (c) (ws-send! c "You didn't explicitly request a subprotocol"))]]))
```

## Installation

From within DrRacket, install the package `rfc6455`. From the
command-line,

    $ raco pkg install rfc6455

(If you are developing the package from a git checkout, see instead
the `link` and `setup` targets of the Makefile.)

## Documentation

Documentation is provided within Racket's own help system once the
package is installed.

## License

The following text applies to all the code in this package except
files marked "public domain" in the `net/rfc6455/examples` directory:

> Copyright (c) 2013 Tony Garnock-Jones <tonygarnockjones@gmail.com>.
>
> This package is distributed under the GNU Lesser General Public
> License (LGPL). This means that you can link it into proprietary
> applications, provided you follow the rules stated in the LGPL. You
> can also modify this package; if you distribute a modified version,
> you must distribute it under the terms of the LGPL, which in
> particular means that you must release the source code for the
> modified software. See <http://www.gnu.org/licenses/lgpl-3.0.txt>
> for more information.