Build instructions
===================

```
$ cd src
$ make -f makefile.unix USE_UPNP=-
```

See readme-qt.rst for instructions on building Mobilepaycoin QT,
the graphical Mobilepaycoin.


Dependency Build Instructions: Ubuntu & Debian
----------------------------------------------
```
$ sudo apt-get install build-essential libssl-dev libdb++-dev libboost-all-dev libqrencode-dev
```

If using Boost 1.37, append -mt to the boost libraries in the makefile.


Dependency Build Instructions: Gentoo
-------------------------------------
```
$ emerge -av1 --noreplace boost openssl sys-libs/db
$ cd ${Mobilepaycoin_DIR}/src
$ make -f makefile.unix USE_UPNP=-
$ strip mobilepaycoind
```


Notes
-----
The release is built with GCC and then "strip mobilepaycoind" to strip the debug
symbols, which reduces the executable size by about 90%.


miniupnpc
---------
```
$ tar -xzvf miniupnpc-1.6.tar.gz
$ cd miniupnpc-1.6
$ make
$ sudo su
$ make install
```


Berkeley DB
-----------
You need Berkeley DB. If you have to build Berkeley DB yourself:
```
$ ../dist/configure --enable-cxx
$ make
```


Boost
-----
If you need to build Boost yourself:
```
$ sudo su
$ ./bootstrap.sh
$ ./bjam install
```


Security
--------
To help make your Mobilepaycoin installation more secure by making certain attacks impossible to
exploit even if a vulnerability is found, you can take the following measures:

* Position Independent Executable
    Build position independent code to take advantage of Address Space Layout Randomization
    offered by some kernels. An attacker who is able to cause execution of code at an arbitrary
    memory location is thwarted if he doesn't know where anything useful is located.
    The stack and heap are randomly located by default but this allows the code section to be
    randomly located as well.

    On an Amd64 processor where a library was not compiled with -fPIC, this will cause an error
    such as: "relocation R_X86_64_32 against `......' can not be used when making a shared object;"

    To build with PIE, use:
    make -f makefile.unix ... -e PIE=1

    To test that you have built PIE executable, install scanelf, part of paxutils, and use:
    scanelf -e ./mobilepaycoin

    The output should contain:
     TYPE
    ET_DYN

* Non-executable Stack
    If the stack is executable then trivial stack based buffer overflow exploits are possible if
    vulnerable buffers are found. By default, mobilepaycoin should be built with a non-executable stack
    but if one of the libraries it uses asks for an executable stack or someone makes a mistake
    and uses a compiler extension which requires an executable stack, it will silently build an
    executable without the non-executable stack protection.

    To verify that the stack is non-executable after compiling use:
    scanelf -e ./mobilepaycoind

    the output should contain:
    STK/REL/PTL
    RW- R-- RW-

    The STK RW- means that the stack is readable and writeable but not executable.
