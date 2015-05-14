
Title: README

Date: 20150514

<A name="toc1-6" title="qzmq" />
# qzmq

Qzmq provides [Q][q] bindings for [CZMQ][czmq],
the high-level C binding for [ØMQ][zeromq].

Version 3.0.1 of qzmq corresponding to the same version of CZMQ.
Currently qzmq offers a subset of CZMQ APIs.
See the CZMQ Coverage below.

Qzmq is written and maintained by Jay Han (<jay.han@gmail.com>)

Qzmq is hosted at [github][qzmq] and
it uses the [issue tracker][issues] for all issues and comments.

<A name="toc2-21" title="Contents" />
## Contents

&emsp;<a href="#toc2-26">Features of qzmq</a>
&emsp;<a href="#toc2-36">CZMQ Coverage</a>
&emsp;<a href="#toc2-64">Copyright and License</a>
&emsp;<a href="#toc2-73">Files of qzmq</a>
&emsp;<a href="#toc2-85">Building qzmq</a>
&emsp;<a href="#toc2-122">Issues</a>
&emsp;<a href="#toc2-126"></a>

<A name="toc2-26" title="Features of qzmq" />
## Features of qzmq
Qzmq lets Q users write

- concurrent code using multithreads on multicores,
- distributed systems connecting with systems
    - written in other languages,
    - written with many software patterns,
using [CZMQ][czmq] and  [ØMQ][zeromq] developed by the [ØMQ][zeromq] community.

<A name="toc2-36" title="CZMQ Coverage" />
## CZMQ Coverage
Qzmq covers a subset of [CZMQ APIs][czmq-reference].

Mostly implemented as of 20150514:

* [zactor - simple actor framework][zactor]
* [zclock - millisecond clocks and delays][zclock]
* [zsock - working with ZeroMQ sockets (high-level)][zsock]
* [zfile - provides methods to work with files in a portable fashion][zfile]
* [zframe - working with single message frames][zframe]
* [zmsg - working with multipart messages][zmsg]
* [zstr - sending and receiving strings][zstr]

Deprecated:

* [zctx - working with ZeroMQ contexts][zctx]
* [zthread - working with system threads][zthread]

May implement the following in the future:

* zloop
* zmonitor
* zpoller

Some czmq APIs seem redundant viz-a-viz q:
zuuid or UUID support, zlist or generic container, etc.

<A name="toc2-64" title="Copyright and License" />
## Copyright and License

    // Copyright (c) 2012-2015 Jaeheum Han <jay.han@gmail.com>
    // This file is part of qzmq.
    // This Source Code Form is subject to the terms of the Mozilla Public
    // License, v. 2.0. If a copy of the MPL was not distributed with this
    // file, You can obtain one at http://mozilla.org/MPL/2.0/.

<A name="toc2-73" title="Files of qzmq" />
## Files of qzmq

* README.md -- README.txt in Markdown syntax thanks to [Gitdown][gitdown].
* README.txt -- this file.
* kx/* -- `k.h` and `c.o` from kx.com for building qzmq. (see [kx.com terms and conditions][kxtoc])
* qzmq.c -- C bindings to be linked with CZMQ (with ØMQ) and Q (`k.h` and `c.o`).
* qzmq.q -- q code used to load the bindings.
* qzmq.so -- dynamic library to be loaded into q by `qzmq.q`.
* qzmq_test.q -- test code translated to q from CZMQ's self-test code.
* zactor_test.q -- zactor example usage.

<A name="toc2-85" title="Building qzmq" />
## Building qzmq
Prerequisites: kdb+ 3.0 or later, [ØMQ][zeromq] 4, [CZMQ][czmq] 3.
Consult your local sysadmin for installation prerequisites.

Current version 3.0.1 of qzmq has been built with 32-bit kdb+ on Ubuntu 14.04.
Instructions for builidng qzmq assumes kdb+ is instaled under `$HOME/q/` directory.

    # for Debian, Ubuntu, ... (kdb+ v3.0 "l32") with ZeroMQ & CZMQ
    # install zeromq 4 (32-bit version)
    sudo aptitude install libzmq3-dev:i386
    # build czmq 32-bit version from github, install to ~/.czmq
    git clone git://github.com/zeromq/czmq
    cd czmq
    ./autogen.sh
    ./configure CFLAGS=-m32 --exec-prefix=$HOME/.czmq --prefix=$HOME/.czmq
    git clone git://github.com/jaeheum/qzmq
    cd qzmq 
    gcc  -DKXVER=3 \
        -m32 \
        -shared -fPIC qzmq.c -o $HOME/q/l32/qzmq.so \
        -Wall -Wextra  -Wl,-rpath -Wl,/usr/lib/i386-linux-gnu  -Wl,-rpath -Wl,$HOME/.czmq/lib \
        -I./kx/kdb+3.0  -I/usr/include/ -I$HOME/.czmq/include \
        -L./kx/kdb+3.0/l32 -L/usr/local/lib -L$HOME/.czmq/lib -lzmq -lczmq
    cp qzmq.q assert.q $HOME/q/

For 64-bit kdb+, install `libzmq3-dev`,  drop `-m32` gcc flag and
replace `l32` architecture path to appropriate one like `l64`.

`qzmq_test.q` contains qzmq translation of czmq's self-tests.
It can be used as examples of each API.

ZeroMQ [Guide][zguide] has examples in many languages.
Translation of these examples to q is [under way][zguide-in-q]
(pulled from [here][zguide-in-q-original]).
See [Issue #5](https://github.com/jaeheum/qzmq/issues/5).

<A name="toc2-122" title="Issues" />
## Issues
See the [issue tracker][issues].
<A name="toc2-126" title="" />

---

`README.txt` is written in [Gitdown][gitdown].

[qzmq]: https://github.com/jaeheum/qzmq
[zeromq]: http://www.zeromq.org
[czmq]: http://czmq.zeromq.org
[q]: http://kx.com
[czmq-reference]:http://api.zeromq.org/czmq3-0:czmq
[zactor]:http://api.zeromq.org/CZMQ3-0:zactor
[zclock]:http://api.zeromq.org/CZMQ3-0:zclock
[zfile]:http://api.zeromq.org/CZMQ3-0:zfile
[zsock]:http://api.zeromq.org/CZMQ3-0:zsock
[zstr]:http://api.zeromq.org/CZMQ3-0:zstr
[zmsg]:http://api.zeromq.org/CZMQ3-0:zmsg
[zframe]:http://api.zeromq.org/CZMQ3-0:zframe
[zctx]:http://api.zeromq.org/CZMQ3-0:zctx
[zthread]:http://api.zeromq.org/CZMQ3-0:zthread
[kxtoc]:http://code.kx.com/wiki/TermsAndConditions
[kdbdoc]: http://code.kx.com/wiki/Cookbook/ExtendingWithC
[issues]: https://github.com/jaeheum/qzmq/issues
[zguide]: http://zguide.zeromq.org
[zguide-in-q]:https://github.com/imatix/zguide/tree/master/examples/Q
[zguide-in-q-original]:https://github.com/jaeheum/zguide/tree/master/examples/Q
[gitdown]: https://github.com/imatix/gitdown

