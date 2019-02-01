# TX2
TX2 Ubuntu Install
  
### Iphone hotspot  
> sudo apt-add-repository ppa:pmcenery/ppa  
> sudo apt-get update  
> sudp apt-get install ipheth-utils  
  
### PIP    
> sudo apt-get install update  
> sudo apt-get install upgrade   
> sudo apt-get install python-pip python3-pip  
> pip -V  
> pip3 -V  
  
### OpenSSL   
Official Web: www.openssl.org/source/  
> sudo mkdir openssl  
> sudo wget "OpenSSL File"  
> sudo tar -zxvf "OpenSSL .gz File"  
  
Then move to openssl folder  
> ./config --prefix=/usr/local --openssldir=/usr/local/openssl  
> sudo make  
> sudo make test  
> sudo make install  
  
### Ubuntu Python Update (3.5.2 to 3.6.6)  
> sudo wget Https://www.python.org/ftp/python/3.6.6/Python-3.6.6.tgz  
> sudo tar -xvf Python-3.6.6.tgz  
  
Then move to Python-3.6.6 folder  
> ./configure --enable-optimizations  
> make  
> sudo make install  
  
Then check python3.6 loc  
> which python3.6  
return /usr/local/bin/python3.6  
  
> which python3  
return /usr/local/bin/python3  
  
> sudo ln -s /usr/local/bin/python3.6 /usr/local/bin/python3  

### TX2 mode
mode / mode Name / Denver 2 /freq / ARM A57 / GPU Freq
mode 0: max-N / 2 / 2.0 GHz / 4 / 1.30 GHz
mode 1: Max-Q / 0 / 1.2 GHz / 4 / 0.85 GHz
mode 2: Max-P Core-All / 2 / 1.4 GHz / 4 / 1.12 GHz
mode 3: Max-P ARM / 0 / 2.0 GHz / 4 / 1.12 GHz
mode 4: Max-P Denver / 2 / 2.0 GHz / 0 / 1.12 GHz

Default mode 1

Open max freq
> cd ~
> sudo ~/jetson_clocks.sh

Check current mode
> sudo nvpmodel -q verbose

Change mode (example change to mode 0)
> sudo nvpmodel -m 0
