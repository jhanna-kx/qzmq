
<A name="toc1-2" title="qzmq" />
# qzmq

qzmq provides [Q][q] bindings for [CZMQ][czmq], the high-level C binding for [ØMQ][zeromq].

This is version 1.2.0-RC1 of qzmq.

qzmq is written and maintained by Jaeheum Han.

qzmq is hosted at [github][qzmq] and it uses the [issue tracker][issues] for all issues and comments.

<A name="toc2-13" title="Contents" />
## Contents

&emsp;<a href="#toc2-18">License</a>
&emsp;<a href="#toc2-29">Files of qzmq</a>
&emsp;<a href="#toc2-40">Building qzmq</a>
&emsp;<a href="#toc2-52">A Quick Tour of qzmq</a>
&emsp;<a href="#toc2-159">A Longer Tour of qzmq</a>
&emsp;<a href="#toc2-168">Issues</a>
&emsp;<a href="#toc2-172"></a>

<A name="toc2-18" title="License" />
## License

Current version 1.2.0-RC1 of qzmq is licensed under Affero GPL. Different licenses may become possible in the future.

Copyright (c) 2012 Jaeheum Han

This file is part of qzmq.

qzmq is free software: you can redistribute it and/or modify it under the terms of the GNU Affero General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.

<A name="toc2-29" title="Files of qzmq" />
## Files of qzmq

* README.txt -- this file.
* README.md -- README.txt in Markdown syntax thanks to [Gitdown][gitdown].
* qzmq.c -- C bindings to be linked with CZMQ (with ØMQ) and Q (`k.h` and `c.o`).
* qzmq.q -- q code used to load the bindings.
* qzmq_test.q -- test code translated to q from CZMQ's self-test code.
* demos/* -- demos. See demos/README.txt or demos/README.md.
* COPYING -- GNU Affero General Public License.

<A name="toc2-40" title="Building qzmq" />
## Building qzmq
Prerequisites: [ØMQ][zeromq] 2.2, [CZMQ][czmq] 1.2.0 and kdb+ 2.8 `k.h` and `c.o`.

Current version 1.2.0-RC0 of qzmq has been built with 32-bit kdb+ on Mac or "m32", ØMQ 2.2 and CZMQ built from git head:

    gcc -bundle -undefined dynamic_lookup qzmq.c -o $HOME/q/m32/qzmq.so -Wall -Wextra -m32 -I$HOME/include -I/usr/local/include/ -L$HOME/q/m32 -L/usr/local/lib -L$HOME/lib -lzmq -lczmq
    
    gcc -shared -fPIC qzmq.c -o /some/where/q/l32/qzmq.so -L/usr/local/lib -I/usr/local/include -I. -Wl,-rpath -Wl,/usr/local/lib # for linux
    
See [kdb+ documentation][kdbdoc] for more details.

<A name="toc2-52" title="A Quick Tour of qzmq" />
## A Quick Tour of qzmq

Load qzmq; learn to read online documentation; write multi-threading code in q; ...

    q)\l qzmq.q  / loads various czmq APIs named z* and libzmq; also creates .qzmq.doc.
    q)\v
    `libzmq`zclock`zctx`zfile`zframe`zloop`zmq`zmsg`zsocket`zsockopt`zstr`zthread
    q)type libzmq  / is for a few helper functions in the low level zeromq API.
    99h
    q)libzmq  / has two functions, version and device.
    version| code
    device | code
    q)type .qzmq.doc / is a 99h that can be queried
    99h
    q)meta .qzmq.doc
    c   | t f a
    ----| -----
    name| s    
    args| s    
    doc | s    
    q).qzmq.doc
    name              | args  doc                                                ..
    ------------------| ---------------------------------------------------------..
    qzmq              |       see https://github.com/jaeheum/qzmq/blob/master/REA..
    czmq              |       see http://czmq.zeromq.org or 'man czmq'.          ..
    zmq               |       see zmq, czmq z* api man pages, `zsockopt and `zsoc..
    zsockopt          |       see http://czmq.zeromq.org/manual:zsockopt or 'man ..
    zhash             |       nyi.                                               ..
    zlist             |       nyi.                                               ..
    zclock            |       see http://czmq.zeromq.org/manual:zclock or 'man zc..
    zclock.sleep      | [x]   sleeps for x milliseconds (-6h).                   ..
    zclock.time       | []    returns the current timestamp in milliseconds (-7h)..
    zclock.log        | [x]   prints YY-mm-dd, followed by x (10h).              ..
    ....
    q).qzmq.doc`libzmq  / shows the documentation hyperlink.
    args| 
    doc | see http://api.zeromq.org or 'man zmq'.
    q).qzmq.doc`libzmq.version  / shows documentation of a particular function.
    args| []
    doc | returns major, minor, patch version numbers (6h) of libzmq.
    q)libzmq.version[]  / returns major, minor, patch version numbers (6h) of libzmq.
    2 2 0
    q).qzmq.doc`zclock
    args| 
    doc | see http://czmq.zeromq.org/manual:zclock or 'man zclock'.
    q).qzmq.doc`zclock.sleep
    args| [x]
    doc | sleeps for x milliseconds (-6h).
    q)select from .qzmq.doc where name like "zthread*"
    name        | args    doc                                                    ..
    ------------| ---------------------------------------------------------------..
    zthread     |         see http://czmq.zeromq.org/manual:zthread or 'man zthre..
    zthread.new | [x;y]   creates a detached thread running `x[y].               ..
    zthread.fork| [x;y;z] creates an attached thread running `y[z] in the zctx x...
    q) / launch some detached threads
    q)dthr:{zclock.log x; zclock.sleep 1000*1000; :0N}
    q)i:0; do[10; zthread.new[`dthr; "tid",string i]; i+:1]
    12-10-05 23:31:22 tid1
    12-10-05 23:31:22 tid0
    12-10-05 23:31:22 tid2
    12-10-05 23:31:22 tid3
    12-10-05 23:31:22 tid4
    12-10-05 23:31:22 tid5
    12-10-05 23:31:22 tid6
    12-10-05 23:31:22 tid7
    12-10-05 23:31:22 tid8
    q)12-10-05 23:31:22 tid9
    q) / check there are ten threads under q on a linux machine
    q)\pstree
    "init-+-atd"
    "     |-auditd---{auditd}"
    "     |-crond"
    "     |-dbus-daemon"
    "     |-2*[dhclient]"
    "     |-6*[mingetty]"
    "     |-ntpd"
    "     |-rsyslogd---2*[{rsyslogd}]"
    "     |-sshd---sshd---sshd---bash---q-+-sh---pstree"
    "     |                               `-10*[{q}]"
    "     `-udevd"
    q) / check there are ten threads under q on a mac os x
    q)\ps -o ppid -M
    " PPID USER   PID   TT   %CPU STAT PRI     STIME     UTIME COMMAND"
    "  349 hjh    171 s000    0.0 S    31T   0:00.03   0:00.02 rlwrap /Users/hjh/..
    "  347 hjh    349 s000    0.0 S    31T   0:03.05   0:04.63 -bash"
    "21825 hjh  21826 s001    0.0 S    31T   0:00.40   0:00.54 -bash"
    "56075 hjh  21989 s002    0.0 S    31T   0:00.05   0:00.89 swipl"
    "56074 hjh  56075 s002    0.0 S    31T   0:02.43   0:03.69 -bash"
    " 4928 hjh    611 s003    0.0 S    31T   0:00.01   0:00.01 rlwrap /Users/hjh/..
    " 4927 hjh   4928 s003    0.0 S    31T   0:00.99   0:01.73 -bash"
    "  611 hjh    612 s004    0.0 S    31T   0:00.01   0:00.02 /Users/hjh/q/m32/q"
    "  611        612         0.0 S    31T   0:00.00   0:00.00 "
    "  611        612         0.0 S    31T   0:00.00   0:00.00 "
    "  611        612         0.0 S    31T   0:00.00   0:00.00 "
    "  611        612         0.0 S    31T   0:00.00   0:00.00 "
    "  611        612         0.0 S    31T   0:00.00   0:00.00 "
    "  611        612         0.0 S    31T   0:00.00   0:00.00 "
    "  611        612         0.0 S    31T   0:00.00   0:00.00 "
    "  611        612         0.0 S    31T   0:00.00   0:00.00 "
    "  611        612         0.0 S    31T   0:00.00   0:00.00 "
    "  611        612         0.0 S    31T   0:00.00   0:00.00 "
    "  612 hjh    622 s004    0.0 S    31T   0:00.00   0:00.00 sh -c ps -o ppid -..
    "  171 hjh    172 s005    0.0 S    31T   0:00.06   0:00.04 /Users/hjh/q/m32/q"
    "  171        172         0.0 S    31T   0:00.00   0:00.00 "
    ..

<A name="toc2-159" title="A Longer Tour of qzmq" />
## A Longer Tour of qzmq

`qzmq_test.q` contains qzmq translation of czmq's self-tests. It can be used as examples of each API.

Code in `demos/*` shows cross-language clients and servers that are interchangeable.

ZeroMQ [Guide][zguide] has examples in many languages. Translation of these examples to q is [under way](https://github.com/imatix/zguide/tree/master/examples/Q)  (pulled from [here](https://github.com/jaeheum/zguide/tree/master/examples/Q)). See [Issue #5](https://github.com/jaeheum/qzmq/issues/5).

<A name="toc2-168" title="Issues" />
## Issues
See the [issue tracker][issues].
<A name="toc2-172" title="" />

---

`README.txt` is written in [Gitdown][gitdown].

[qzmq]: https://github.com/jaeheum/qzmq
[zeromq]: http://www.zeromq.org
[czmq]: http://czmq.zeromq.org
[q]: http://kx.com
[kdbdoc]: http://code.kx.com/wiki/Cookbook/ExtendingWithC
[issues]: https://github.com/jaeheum/qzmq/issues
[zguide]: http://zguide.zeromq.org
[gitdown]: https://github.com/imatix/gitdown
