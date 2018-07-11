### **Code (1) - install bitcoin**



### (1) runtime environment
```
~:sudo apt-get install build-essential libtool autotools-dev autoconf automake libssl-dev libboost-all-dev libdb-dev libdb++-dev pkg-config libevent-dev git-core
```
### (2) clone from git
```
```

### (3)  compile and install
```
~:cd bitcoin
~:./autogen.sh

~:./configure --without-gui
# configure: error: Found Berkeley DB other than 4.8, required for portable wallets
# if you meet this error use the following command
~:./configure --without-gui --disable-wallet

~:make -j
~:make install
```

