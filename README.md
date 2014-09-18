# Getting Linux to work at VHT

## Nameservice Switching

In order to resolve domains that end in .local, change the following line in /etc/nsswitch.conf

```
hosts: files mdns4_minimal [NOTFOUND=return] dns
```

to read

```
hosts: files mdns4_minimal dns
```

## Additional Search Domains

You need to add `qalab.local, devlab.local` to the Additional Search Domains entry in the IPv4 Settings for your network connection.

![search_domains.png](images/search_domains.png)

## FreeTDS ODBC Driver

FreeTDS is an open source driver to connect to SQL Server (and, by extension, Sybase) databases. The ODBC driver for it allows the unixodbc system to connect to SQL Server. This, in turn, enables Erlang's odbc module to connect to our SQL Server databases. Install the TDS ODBC driver by typing

```
sudo apt-get install tdsodbc
```

Then identify the actual driver to use. Also, provide a better default protocol version (that actaully works with SQL Server). Update your /etc/odbcinst.ini file with the new driver, as

```
[FreeTDS]
Driver = /usr/lib/x86_64-linux-gnu/odbc/libtdsodbc.so
TDS Version = 7.2
```

## General Compilation

`sudo apt-get install build-essential`

## Build Erlang

Acquire the `make_erlang_alternative` script.
Download **OTP R16B03-1** (src and doc_man) from [the Erlang web site](http://erlang.org), as build with the following recipe

```
sudo apt-get build-dep erlang # installs the build dependencies
sudo apt-get install wxgtk2.8-dev # dependency that only applies to graphical windows
mkdir ~/src
cd ~/src
tar -xzf ../Downloads/otp_src_R16B03-1.tar.gz
./configure --prefix=/opt/erlang/R16B03-1
make
sudo make install
cd /opt/erlang/R16B03-1/lib/erlang/erts-5.10.4/man
tar -xz --strip-components=1 -f ~/Downloads/otp_doc_man_R16B03-1.tar.gz
cd
`bin/make_erlang_alternative /opt/erlang/R16B03-1 510` # 510 is the priority
```

You can, later, install other versions of erlang and use the alternative scripts to quickly switch betweeen them.

## Build NodeJS

Download NodeJS from [the NodeJS web site](http://nodejs.org) and build into `/usr/local`

```
cd ~/src
tar xzf ~/Downloads/nodejs-*.tar.gz
./configure
make
sudo make install
```

Get the needed global packages installed

```
npm install -g grunt-cli
npm install -g bower
```

You should see executables in `/usr/local/bin` for `node`, `npm`, `grunt`, and `bower`.

## RVM and Ruby

Install [RVM](http://rvm.io) from the web.

```
sudo apt-get install curl git-core
\curl -sSL https://get.rvm.io | bash -s stable
rvm install 1.9
```

Update your gnome-terminal settings to `run command as a login shell`. Restart the terminal. RVM should now work, so you can install ruby.

```
rvm install 1.9
```
