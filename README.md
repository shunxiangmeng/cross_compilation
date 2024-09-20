# cross_compilation
cross compilation third lib

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
make
make install
```

# curl
1. no ssl

```
./configure --host=arm-linux CC=arm-buildroot-linux-uclibcgnueabihf-gcc --without-ssl --without-zlib --prefix=/home/test/opensource/curl-8.2.1/output/arm-buildroot-linux-uclibcgnueabihf --enable-static
```

# openssl
```
wget https://www.openssl.org/source/openssl-3.0.0.tar.gz
tar -xzf openssl-3.0.0.tar.gz
cd openssl-3.0.0

./Configure no-asm shared no-async --prefix=/usr/local/arm-openssl-3.0 CROSS_COMPILE=/opt/rockchip/arm-rockchip830-linux-uclibcgnueabihf/bin/arm-rockchip830-linux-uclibcgnueabihf-
make 
make install

# no-asm： 是在交叉编译过程中不使用汇编代码代码加速编译过程，原因是它的汇编代码是对arm格式不支持的。
# shared ：生成动态连接库。
# no-async：没有提供GNU C的ucontext库。
# --prefix：安装目录
# CROSS_COMPILE：交叉编译工具链前缀
```

如果编译出现 -m64错误，删除Makefile中全部 -m64即可