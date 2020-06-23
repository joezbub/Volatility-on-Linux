# How to Install Volatility on Linux
(https://www.volatilityfoundation.org)[Volatility] is a powerful tool used for analyzing memory dumps on Linux, Mac, and Windows systems. On Linux and Mac systems, one has to build profiles separately, and notably, they must match the memory system profile (building a Ubuntu 18.04.3 profile to analyze a Ubuntu 18.04.4 system will not work). This article will go over all the dependencies that need to be downloaded as well as how to build a profile.

# Dependencies
Note that information is taken from [Volatility's Github](https://github.com/volatilityfoundation/volatility/wiki/Installation). Many Volatility plugins will not work with the following packages.

## Prerequisites for Dependencies
Run the following:
<pre><code>
$ sudo apt-get install git subversion pcregrep libpcre++-dev python-dev -y
$ sudo apt-get install build-essential -y
$ sudo apt-get install dwarfdump -y
</code></pre>

## Install Distorm
Do not use `pip install distorm3`. It will not build correctly. Instead, download the source tar from [here](https://github.com/gdabah/distorm/releases). After unzipping it, cd into the distorm3/ and run:
<pre><code>
$ python setup.py build
$ sudo python setup.py build install
</code></pre>

## Install Yara
Do not use `pip install distorm3`. I made this mistake, and Volatility was not able to detect Yara. Make sure to go to the [main website](https://github.com/VirusTotal/yara/releases), and download the source tar. Run:
<pre><code>
$ tar -zxf yara-4.0.1.tar.gz
$ cd yara-4.0.1
$ ./bootstrap.sh
</code></pre>

Install some dependencies:
<pre><code>
$ sudo apt-get install automake libtool make gcc pkg-config</code></pre>

Continue:
<pre><code>
$ ./configure
$ make
$ sudo make install
</code></pre>

Check to see if it installed properly:
<pre><code>
$ make check
</code></pre>

## Install PyCrypto
Download the latest source from [here](https://www.dlitz.net/software/pycrypto/)
<pre><code>
$ tar -zxvf pycrypto-2.6.1.tar.gz
$ python setup.py build
$ sudo python setup.py build install
</code></pre>

## Install Volatility
Finally, clone from Volatility's Github repo and install:
<pre><code>
$ git clone https://github.com/volatilityfoundation/volatility.git
$ cd volatility
$ python setup.py
</code></pre>