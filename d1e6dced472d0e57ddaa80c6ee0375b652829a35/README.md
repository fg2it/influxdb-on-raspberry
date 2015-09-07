# Influxdb for raspberry pi
build from [d1e6dce](https://github.com/influxdb/influxdb/tree/d1e6dced472d0e57ddaa80c6ee0375b652829a35/)
commit without any change.

Building influxdb is becoming increasingly easy, you just have to follow
official [instructions](https://github.com/influxdb/influxdb/blob/d1e6dced472d0e57ddaa80c6ee0375b652829a35/CONTRIBUTING.md). 

```bash
cd
#install gvm
sudo apt-get install bison
bash < <(curl -s -S -L https://raw.githubusercontent.com/moovweb/gvm/master/binscripts/gvm-installer)
source ~/.gvm/scripts/gvm
gvm install go1.4.2
gvm use go1.4.2
#for go1.5
GOROOT_BOOTSTRAP=~/.gvm/gos/go1.4.2 gvm install go1.5
gvm use go1.5
#install fpm
sudo apt-get install ruby-dev
sudo gem install fpm
#install rpm (for rpmbuild) if needed
#sudo apt-get install rpm

```

```bash
gvm use go1.5
export GOPATH=$HOME/gocodez
mkdir -p $GOPATH/src/github.com/influxdb
cd $GOPATH/src/github.com/influxdb
git clone https://github.com/influxdb/influxdb # < 40s
cd influxdb
git checkout d1e6dced472d0e57ddaa80c6ee0375b652829a35
./package.sh -p -t deb 0.9.4                   # run for 10m
```

You end up with `influxdb_0.9.4_armhf.deb` in `$GOPATH/src/github.com/influxdb`.
