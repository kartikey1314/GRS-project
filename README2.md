ngtcp2
======

## Building with wolfSSL

```shell
$ git clone --depth 1 -b v5.7.6-stable https://github.com/wolfSSL/wolfssl
$ cd wolfssl
$ autoreconf -i
$ # For wolfSSL < v5.6.6, append --enable-quic.
$ ./configure --prefix=$PWD/build \
--enable-all --enable-aesni --enable-harden --enable-keylog-export \
--disable-ech
$ make -j$(nproc)
$ make install
$ cd ..

$ git clone --recursive https://github.com/ngtcp2/nghttp3
$ cd nghttp3
$ autoreconf -i
$ ./configure --prefix=$PWD/build --enable-lib-only
$ make -j$(nproc) check
$ make install
$ cd ..

$ git clone --recursive https://github.com/ngtcp2/ngtcp2
$ cd ngtcp2
$ autoreconf -i
$ # For Mac users who have installed libev with MacPorts, append
$ # LIBEV_CFLAGS="-I/opt/local/include" LIBEV_LIBS="-L/opt/local/lib -lev"
$ ./configure PKG_CONFIG_PATH=$PWD/../wolfssl/build/lib/pkgconfig:$PWD/../nghttp3/build/lib/pkgconfig \
--with-wolfssl
$ make -j$(nproc) check


Setting up Server
-------------

use command 

Server

```bash
./wsslserver -d /mnt/myssd/ngtcp2/www --htdocs
/mnt/myssd/ngtcp2/www 0.0.0.0 4433
/mnt/myssd/ngtcp2/wolfssl/certs/server-key.pem
/mnt/myssd/ngtcp2/wolfssl/certs/server-cert.pem
```

Client
~~~~~~

```bash
./wsslclient 127.0.0.1 4433 https://localhost:4433/100MBfile.bin
--key=/mnt/myssd/ngtcp2/
wolfssl/certs/client-key.pem
--cert=/mnt/myssd/ngtcp2/
wolfssl/certs/client-cert.pem --download=downloaded_file.bin
```
