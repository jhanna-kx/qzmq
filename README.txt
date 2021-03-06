.set GIT=https://github.com/jaeheum/qzmq
.set PREVER=3.0.1
.set CZMQVER=3.0.2
.set ZMQVER=4.1.4

Title: README

Date: &date()

# qzmq

Qzmq provides [Q][q] bindings for [CZMQ][czmq],
the high-level C binding for [ØMQ (ZeroMQ)][zeromq].

Version $(CZMQVER) of qzmq corresponding to the same version of CZMQ.
Qzmq offers a subset of CZMQ APIs.
See the CZMQ Coverage below.

Qzmq is written and maintained by Jay Han (<jay.han@gmail.com>)

Qzmq is hosted at [github][qzmq] and
it uses the [issue tracker][issues] for all issues and comments.

## Contents

.toc 1

## News
This release of qzmq is different from 20150518 ($(PREVER)) release:

- deprecated `zactor` or concurrency support
- deprecated `zclock`, `zctx`, `zfile`
- removed helper methods `zmsg_{load,save}`
- built/tested on CentOS 7 with OS supplied CZMQ ($(CZMQVER)) and ØMQ ($(ZMQVER)) libraries

## Features of qzmq
Qzmq lets Q users write

- distributed systems connecting with systems
    - written in other languages,
    - written with many software patterns,
using [CZMQ][czmq] and [ØMQ/ZeroMQ][zeromq] developed by the [ØMQ][zeromq] community.

[ZeroMQ][zeromq] takes care of data mobility/distribution/topology
(e.g. pub/sub, req/rep, etc.) over various networks and transports
(threads/processes/hosts). Note that threads are not supported
by qzmq.

Because q has first-class functions qzmq can move code as well as data.


## CZMQ Coverage
Qzmq covers a subset of [CZMQ APIs][czmq-reference].

Mostly implemented as of &date():

* [zsock - working with ZeroMQ sockets (high-level)][zsock]
* [zframe - working with single message frames][zframe]
* [zmsg - working with multipart messages][zmsg]
* [zstr - sending and receiving strings][zstr]

Deprecated:

* [zactor - simple actor framework][zactor]
* [zclock - millisecond clocks and delays][zclock]
* [zctx - working with ZeroMQ contexts][zctx]
* [zfile - provides methods to work with files in a portable fashion][zfile]
* [zthread - working with system threads][zthread]

May implement the following in the future:

* zloop
* zmonitor
* zpoller

Some czmq APIs seem redundant viz-a-viz q:
zuuid or UUID support, zlist or generic container, etc.

## Copyright and License

    // Copyright (c) 2012-2016 Jaeheum Han <jay.han@gmail.com>
    // This file is part of qzmq.
    // This Source Code Form is subject to the terms of the Mozilla Public
    // License, v. 2.0. If a copy of the MPL was not distributed with this
    // file, You can obtain one at http://mozilla.org/MPL/2.0/.

## Files of qzmq

* README.md -- README.txt in Markdown syntax thanks to [Gitdown][gitdown].
* README.txt -- this file.
* kx/* -- `k.h` and `c.o` from kx.com for building qzmq. (see [kx.com terms and conditions][kxtoc])
* qzmq.c -- C bindings to be linked with CZMQ (with ØMQ) and Q (`k.h` and `c.o`).
* qzmq.q -- q code used to load the bindings.
* qzmq.so -- dynamic library to be loaded into q by `qzmq.q`.

## Building qzmq
Prerequisites: kdb+ 3.0 or later, [ØMQ][zeromq] 4, [CZMQ][czmq] 3.
Consult your local sysadmin for installation prerequisites.

Current version $(CZMQVER) of qzmq has been built with 64-bit kdb+ on CentOS 7.
Instructions for builidng qzmq assumes `kdb+` is in `$HOME/q/` directory.

    git clone git://github.com/jaeheum/qzmq
    cd qzmq
    gcc -DKXVER=3 \
        -shared -fPIC qzmq.c -o $HOME/q/l64/qzmq.so \
        -Wall -Wextra \
        -Wl,-rpath -Wl,/usr/lib64 \
        -I./kx/kdb+3.0 \
        -L./kx/kdb+3.0/l64 \
        `pkg-config --cflags --libs czmq`
    cp qzmq.q assert.q $HOME/q/

## Running qzmq
    
    \l qzmq.q

See `qzmq_test.q` for examples.

* `qzmq_test.q` -- qzmq translation of CZMQ's self-tests.

ZeroMQ [Guide][zguide] has examples in many languages.
Translation of these examples to q is [under way][zguide-in-q]
(pulled from [here][zguide-in-q-original]).
See [Issue #5](https://github.com/jaeheum/qzmq/issues/5).

## Issues
See the [issue tracker][issues].

---

`README.txt` is written in [Gitdown][gitdown].

.- Reference
.-
[qzmq]: $(GIT)
[zeromq]: http://www.zeromq.org
[czmq]: http://czmq.zeromq.org
[q]: http://kx.com
[czmq-reference]:http://api.zeromq.org/czmq3-0:czmq
[zsock]:http://api.zeromq.org/CZMQ3-0:zsock
[zstr]:http://api.zeromq.org/CZMQ3-0:zstr
[zmsg]:http://api.zeromq.org/CZMQ3-0:zmsg
[zframe]:http://api.zeromq.org/CZMQ3-0:zframe
[kxtoc]:http://code.kx.com/wiki/TermsAndConditions
[kdbdoc]: http://code.kx.com/wiki/Cookbook/ExtendingWithC
[issues]: $(GIT)/issues
[zguide]: http://zguide.zeromq.org
[zguide-in-q]:https://github.com/imatix/zguide/tree/master/examples/Q
[zguide-in-q-original]:https://github.com/jaeheum/zguide/tree/master/examples/Q
[gitdown]: https://github.com/zeromq/gitdown

