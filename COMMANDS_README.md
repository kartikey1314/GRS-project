![quiche](quiche.svg)



### curl

quiche can be [integrated into curl][curl-http3] to provide support for HTTP/3.
[curl-http3]: https://github.com/curl/curl/blob/master/docs/HTTP3.md#quiche-version

Getting Started
---------------


### HTTP/3

The quiche [HTTP/3 module] provides a high level API for sending and
receiving HTTP requests and responses on top of the QUIC transport protocol.

Building quiche from C/C++
-------------------------


When running ``cargo build``, a static library called ``libquiche.a`` will be
built automatically alongside the Rust one. This is fully stand-alone and can
be linked directly into C/C++ applications.

Building
--------

quiche requires Rust 1.81 or later to build. The latest stable Rust release can
be installed using [rustup](https://rustup.rs/).

Once the Rust build environment is setup, the quiche source code can be fetched
using git:

```bash
 $ git clone --recursive https://github.com/cloudflare/quiche
```

and then built using cargo:

```bash
 $ cargo build --examples
```

cargo can also be used to run the testsuite:

```bash
 $ cargo test
```

Note that [BoringSSL], which is used to implement QUIC's cryptographic handshake
based on TLS, needs to be built and linked to quiche. This is done automatically
when building quiche using cargo, but requires the `cmake` command to be
available during the build process.


```bash
 $ QUICHE_BSSL_PATH="/path/to/boringssl" cargo build --examples
```

Alternatively you can use [OpenSSL/quictls]. To enable quiche to use this vendor
the ``openssl`` feature can be added to the ``--feature`` list. Be aware that
``0-RTT`` is not supported if this vendor is used.

[BoringSSL]: https://boringssl.googlesource.com/boringssl/

[OpenSSL/quictls]: https://github.com/quictls/openssl

# Quiche HTTP/3 Server

This server is built using [Cloudflare's quiche](https://github.com/cloudflare/quiche), a Rust implementation of the QUIC transport protocol and HTTP/3.

## 🛠 Build Instructions

Make sure you have Rust installed. Then, clone the repository and build the server:

```bash
RUST_LOG=debug ./quiche-server --cert
~/quiche/tools/testserver/cert.pem
--key ~/quiche/tools/testserver/key.pem
--listen 0.0.0.0:4433
--root ~/quiche/tools/testserver/
```

 Running Client
Use the command to fetch a file from a running HTTP/3 server:

```bash
./quiche-client --no-verify
https://127.0.0.1:4433/
100MBfile.bin > quiche_downloaded.bin
```


