# How to Install Volatility on Linux
[Volatility](https://www.volatilityfoundation.org) is a powerful tool used for analyzing memory dumps on Linux, Mac, and Windows systems. On Linux and Mac systems, one has to build profiles separately, and notably, they must match the memory system profile (building a Ubuntu 18.04.3 profile to analyze a Ubuntu 18.04.4 system will not work). This article will go over all the dependencies that need to be downloaded as well as how to build a profile.

# Dependencies
Note that information is taken from [Volatility's Github](https://github.com/volatilityfoundation/volatility/wiki/Installation). Many Volatility plugins will not work with the following packages.

## Prerequisites for Dependencies
Run the following:
<pre><code>$ sudo apt-get install build-essential autoconf dwarfdump git subversion pcregrep libpcre++-dev python-dev -y
</code></pre>

## Install Distorm
Do not use `pip install distorm3`. It will not build correctly. Instead, download the source tar from [here](https://github.com/gdabah/distorm/releases). After unzipping it, cd into the the directory and run:
<pre><code>$ python setup.py build
$ sudo python setup.py build install
</code></pre>

## Install Yara
Do not use `pip install yara-python`. I made this mistake, and Volatility was not able to detect Yara. Make sure to go to the [main website](https://github.com/VirusTotal/yara/releases), and download the source tar. Run:
<pre><code>$ tar -zxf yara-4.0.1.tar.gz
$ cd yara-4.0.1
$ ./bootstrap.sh
</code></pre>

Install some dependencies:
<pre><code>$ sudo apt-get install automake libtool make gcc pkg-config</code></pre>

Continue:
<pre><code>$ ./configure
$ make
$ sudo make install
</code></pre>

Check to see if it installed properly:
<pre><code>$ make check
</code></pre>

## Install PyCrypto
Download the latest source from [here](https://www.dlitz.net/software/pycrypto/)
<pre><code>$ tar -zxvf pycrypto-2.6.1.tar.gz
$ python setup.py build
$ sudo python setup.py build install
</code></pre>

## Install Volatility
Finally, clone from Volatility's Github repo and install:
<pre><code>$ git clone https://github.com/volatilityfoundation/volatility.git
$ cd volatility
$ sudo python setup.py build install
</code></pre>

Create module.dwarf:
<pre><code>$ cd volatility/tools/linux
$ make
</code></pre>

Make a zip containing module.dwarf and the exact profile of your Linux distro:
<pre><code>$ cd ../../../
$ sudo zip $(lsb_release -i -s)_$(uname -r)_profile.zip ./volatility/tools/linux/module.dwarf /boot/System.map-$(uname -r)
</code></pre>

Copy the zip file into the Volatility plugin path:
<pre><code>$ cp *name*.zip ./volatility/volatility/plugins/overlays/linux
</code></pre>

Test if installation is complete and profile is configured:
<pre><code>$ cd volatility
$ python vol.py --info | grep $(lsb_release -i -s)
</code></pre>

# Generating a Memory Sample with LiME
LiME is a memory acquisition tool made specifically for Linux devices.

## Dependencies
<pre><code>$ sudo apt install linux-headers-4.9.0-8-amd64
$ sudo apt install build-essential
</code></pre>

## Download and Compile
Install the latest version:
<pre><code>$ git clone https://github.com/504ensicsLabs/LiME
</code></pre>  

Compile:
<pre><code>$ cd LiME/src/
$ make
</code></pre>
A file named `lime-5.3.0-62-generic.ko` is created.

## Taking Memory Image
Use insmod to load the compiled LKM. Also, `format=lime` and `timeout=0` are imp
ortant for analysis via Volatility.
<pre><code>$ sudo insmod lime-5.3.0-62-generic.ko "path=/home/dump1.mem format=lime timeout=0"
</code></pre>

 
