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

