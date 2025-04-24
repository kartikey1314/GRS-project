
For building LSQUIC, we need CMake, zlib, and BoringSSL.  The example program
uses libevent to provide the event loop.

Building BoringSSL
------------------

BoringSSL is not packaged; we have to build it yourself.  The process is
straightforward.  You will need `go` installed.

1. Clone BoringSSL:

```
git clone https://boringssl.googlesource.com/boringssl
cd boringssl
```

We need to install pre-requisites like zlib and libevent.

2. Use specific BoringSSL version

```
git checkout 9fc1c33e9c21439ce5f87855a6591a9324e569fd
```

3. Compile the library

```
cmake . &&  make
```

Remember where BoringSSL sources are:
```
BORINGSSL=$PWD
```

If you want to turn on optimizations, do

```
cmake -DCMAKE_BUILD_TYPE=Release . && make
```

If you want to build as a library, (necessary to build lsquic itself
as as shared library) do:

```
cmake -DBUILD_SHARED_LIBS=1 . && make
```

Building LSQUIC Library
-----------------------

LSQUIC's `http_client`, `http_server`, and the tests link BoringSSL
libraries statically.  Following previous section, you can build LSQUIC
as follows:

1. Get the source code

```
git clone https://github.com/litespeedtech/lsquic.git
cd lsquic
git submodule update --init
```

2. Compile the library

Statically:


```
# $BORINGSSL is the top-level BoringSSL directory from the previous step
cmake -DBORINGSSL_DIR=$BORINGSSL .
make
```

As a dynamic library:

```
cmake -DLSQUIC_SHARED_LIB=1 -DBORINGSSL_DIR=$BORINGSSL .
make
```


3. Run tests

```
make test
```


# QUIC Server and Client Execution

This explains how to run the LSQUIC-based server:

---

## 🔧 Server Setup

To start the server, run command:

```bash
./wsslserver -d /mnt/myssd/ngtcp2/www \
  --htdocs /mnt/myssd/ngtcp2/www \
  0.0.0.0 4433 \
  <path-to-server-key.pem> \
  <path-to-server-cert.pem>
```

This explains how to run the LSQUIC-based  client: 


## 🔧 Client Setup


```bash
./wsslclient https://localhost:4433/1mb.txt \
  --cert=<path-to-client-cert.pem> \
  --key=<path-to-client-key.pem>
```
