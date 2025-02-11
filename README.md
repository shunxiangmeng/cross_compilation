# cross_compilation
cross compilation third lib

# zlib
1. 下载地址： https://www.zlib.net/

CC=arm-linux-gnueabihf-gcc ./configure --prefix=/path/to/install/arm/zlib

# tcpdump
tcpdump 依赖于 libpcap，需要为目标平台交叉编译 libpcap

1. 下载源码
```
wget http://www.tcpdump.org/release/tcpdump-4.9.1.tar.gz
tar -zxvf tcpdump-4.9.1.tar.gz
wget http://www.tcpdump.org/release/libpcap-1.8.1.tar.gz
tar -zxvf libpcap-1.8.1.tar.gz
```
2. 交叉编译 libpcap
```
cd libpcap-1.8.1
./configure --host=arm-linux-gnueabihf --with-pcap=linux
make
```
3. 交叉编译 tcpdump
```
cd ../tcpdump-4.9.1
./configure --host=arm-linux-gnueabihf CPPFLAGS="-I../libpcap-1.8.1" LDFLAGS="-L../libpcap-1.8.1"
make
arm-linux-gnueabihf-strip tcpdump  #瘦身
```

# sqlite
下载地址：https://www.sqlite.org/ 

编译命令：
```
./configure --host=arm-linux-gnueabihf --prefix=/path/to/install
./configure --host=aarch64-none-linux-gnu --prefix=/home/test/opensource/install/sqlite/aarch64-none-linux-gnu CC=/opt/axera/gcc-arm-9.2-2019.12-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-gcc
make
make install
```

# openssl
```
wget https://www.openssl.org/source/openssl-3.0.0.tar.gz
tar -xzf openssl-3.0.0.tar.gz
cd openssl-3.0.0

./Configure linux-aarch64 no-asm shared no-async --prefix=/home/test/opensource/install/openssl/11091126 CROSS_COMPILE=/opt/rockchip/arm-rockchip830-linux-uclibcgnueabihf/bin/arm-rockchip830-linux-uclibcgnueabihf-
make 
make install

# linux-aarch64: 芯片架构，使用lscpu来查看
# no-asm： 是在交叉编译过程中不使用汇编代码代码加速编译过程，原因是它的汇编代码是对arm格式不支持的。
# shared： 生成动态连接库。
# no-async：没有提供GNU C的ucontext库。
# --prefix：安装目录
# CROSS_COMPILE：交叉编译工具链前缀
```

如果编译出现 -m64错误，删除Makefile中全部 -m64即可

openssl_1.1.1
```
./Configure linux-generic32 no-asm shared no-async --prefix=/home/test/opensource/install/openssl_1.1.1/11091126 CROSS_COMPILE=/opt/rockchip/gcc-arm-8.3-2019.03-x86_64-arm-linux-gnueabihf/bin/arm-linux-gnueabihf-

# aarch64-none-linux-gnu
./Configure linux-aarch64 no-asm shared no-async --prefix=/home/test/opensource/install/openssl_1.1.1/aarch64-none-linux-gnu CROSS_COMPILE=/opt/axera/gcc-arm-9.2-2019.12-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-
```


# wolfssl
# mbedtls
mkdir build
cd build
cmake ../ -DCMAKE_TOOLCHAIN_FILE=/home/test/opensource/toolchain.cmake -DCMAKE_INSTALL_PREFIX=/home/test/opensource/install/mbedtls/11091126

## toolchain.cmake
```
set(CMAKE_SYSTEM_NAME Linux)
set(CMAKE_SYSTEM_PROCESSOR arm)
set(CMAKE_C_COMPILER arm-linux-gnueabihf-gcc)
set(CMAKE_CXX_COMPILER arm-linux-gnueabihf-g++)
# 添加 C99 标准支持
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c99")
set(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)
set(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
set(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)
set(CMAKE_FIND_ROOT_PATH_MODE_PACKAGE ONLY)
```


# curl
1. no ssl

```
./configure --host=arm-linux CC=arm-buildroot-linux-uclibcgnueabihf-gcc --without-ssl --without-zlib --prefix=/home/test/opensource/curl-8.2.1/output/arm-buildroot-linux-uclibcgnueabihf --enable-static
```
2. with ssl
```
# 1109/1126
./configure --host=arm-linux-gnueabihf --prefix=/home/test/opensource/install/curl/11091126 --with-ssl=/home/test/opensource/install/openssl_1.1.1/11091126 CC=/opt/rockchip/gcc-arm-8.3-2019.03-x86_64-arm-linux-gnueabihf/bin/arm-linux-gnueabihf-gcc

# rk3588
./configure --host=aarch64-linux-gnu --prefix=/home/test/opensource/install/curl/aarch64-linux-gnu --with-ssl=/home/test/opensource/install/openssl_1.1.1/aarch64-linux-gnu CC=/opt/sophon/gcc-linaro-7.5.0-2019.12-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-gcc

# axera
./configure --host=aarch64-none-linux-gnu --prefix=/home/test/opensource/install/curl/aarch64-none-linux-gnu  --with-ssl=/home/test/opensource/install/openssl_1.1.1/aarch64-none-linux-gnu CC=/opt/axera/gcc-arm-9.2-2019.12-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-gcc
```

3. with mbedtls
```
./configure --host=arm-linux-gnueabihf --prefix=/home/test/opensource/install/curl/11091126_mbedtls --with-mbedtls=/home/test/opensource/install/mbedtls/11091126 CC=/opt/rockchip/gcc-arm-8.3-2019.03-x86_64-arm-linux-gnueabihf/bin/arm-linux-gnueabihf-gcc --enable-static --disable-shared
```

# iptables
```
git clone git://git.netfilter.org/iptables.git
cd iptables

export CC=arm-linux-gnueabihf-gcc
export CXX=arm-linux-gnueabihf-g++
export AR=arm-linux-gnueabihf-ar
export RANLIB=arm-linux-gnueabihf-ranlib
export STRIP=arm-linux-gnueabihf-strip
./autogen.sh
./configure --host=arm-linux-gnueabihf --prefix=/home/test/opensource/install/iptables/11091126 --enable-static --disable-shared
make
make install
```

# mosquitto
```
export CC=/opt/axera/gcc-arm-9.2-2019.12-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-gcc
export CXX=/opt/axera/gcc-arm-9.2-2019.12-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-g++
export AR=/opt/axera/gcc-arm-9.2-2019.12-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-ar
export RANLIB=/opt/axera/gcc-arm-9.2-2019.12-x86_64-aarch64-none-linux-gnu/bin/aarch64-none-linux-gnu-ranlib

make WITH_SRV=no \
     WITH_WEBSOCKETS=no \
     WITH_CJSON=no \
     LDFLAGS="-L/home/test/opensource/install/openssl_1.1.1/aarch64-none-linux-gnu/lib -lssl -lcrypto" \
     CFLAGS="-I/home/test/opensource/install/openssl_1.1.1/aarch64-none-linux-gnu/include" \
     prefix=/home/test/opensource/install/mosquitto/aarch64-none-linux-gnu 
make install
```