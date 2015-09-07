# Influxdb 0.8.9 release for raspberry pi
Build from [0.8.9](https://github.com/influxdb/influxdb/tree/v0.8.9).

You need to have gcc4.8. It is included in raspbian (2015-05-05) but is not the default.
See for exemple [this](http://lektiondestages.blogspot.fr/2013/05/installing-and-switching-gccg-versions.html)
to know how to change that.

You need to install some dependencies:
```bash
sudo apt-get install mercurial bzr protobuf-compiler flex bison valgrind g++ make autoconf libtool libz-dev libbz2-dev curl rpm build-essential git wget gawk
```

You need go and fpm
```bash
cd
#install gvm
sudo apt-get install
bison bash < <(curl -s -S -L https://raw.githubusercontent.com/moovweb/gvm/master/binscripts/gvm-installer)
source ~/.gvm/scripts/gvm
gvm install go1.4.2
gvm use go1.4.2
#for go1.5
GOROOT_BOOTSTRAP=~/.gvm/gos/go1.4.2 gvm install go1.5
gvm use go1.5
#install fpm
sudo apt-get install ruby-dev
sudo gem install fpm
#install rpm (for rpmbuild) since make package
#build both rpm and deb
sudo apt-get install rpm
```

After that, you are ready to go. For building binaries, everything is almost fine by default, and you merely have to follow the official
[instructions](https://github.com/influxdb/influxdb/blob/v0.8.9/docs/contributing.md).
For packages, a few things need to be fix inside `Makefile`
```bash
cd
gvm use go1.5
export GOPATH=$HOME/gocodez
mkdir -p $GOPATH/src/github.com/influxdb
cd $GOPATH/src/github.com/influxdb
git clone -b v0.8.9 https://github.com/influxdb/influxdb # 40s
go get github.com/golang/protobuf/{proto,protoc-gen-go}  # 35s
cd influxdb
./configure
#no need for rvm so kill that
sed -i 's/rvm use 1.9.3@influxdb &&//' Makefile
#package names are wrong, work around that
sed -i 's/armel[.]rpm/armv7l.rpm/' Makefile
sed -i 's/armel[.]deb/armhf.deb/' Makefile
#here you have to set arch and rocksdb
make arch=arm rocksdb=no version=0.8.9 package      # 18m20
```