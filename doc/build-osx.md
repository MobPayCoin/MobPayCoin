Build instructions
===================

See readme-qt.rst for instructions on building Mobilepaycoin QT, the
graphical user interface.

All of the commands should be executed in Terminal.app

```
sudo port install boost db48 openssl miniupnpc qrencode

cd mobilepaycoin/src
make -f makefile.osx
```

```
./mobilepaycoind --help  # for a list of command-line options.

./mobilepaycoind -daemon # to start the mobilepaycoin daemon.

./mobilepaycoind help # When the daemon is running, to get a list of RPC commands
```
