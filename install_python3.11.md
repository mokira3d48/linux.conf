# How to Install Python 3.11 on Ubuntu 20.04
Python is a high-level, object-oriented and multipurpose programming language that is extremely popular thanks to its simple and easy-to-learn syntax. It is widely used in various IT disciplines such as data analytics and visualization, web development, gaming development, AI, and machine learning. 

## Install Dependencies
Run the below command to update the system and install the required dependencies for the python 3.11.

```sh
sudo apt update
sudo apt install build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev libsqlite3-dev wget libbz2-dev
```

## Download Python 3.11
At the time of writing this article, the latest version of 3.11 is version 3.11.3. We recommend you check Python's official site to get the latest version for your installation, https://www.python.org/downloads/

```sh
wget -c https://www.python.org/ftp/python/3.11.10/Python-3.11.10.tar.xz
```

```sh
tar -xvf Python-3.11.3.tgz
```

> If a different version has been downloaded, ensure you extract the correct file.


## Build and Install Python 3.11 on Ubuntu 20.04

Next, `cd` into the extracted directory, in our case it is `Python-3.11.3` (the name may vary if a different version has been downloaded).

And run the `configure` command to verify if all the dependencies are met for the Python installation.

```sh
cd Python-3.11.3

./configure --enable-optimizations
```

Let's get the number of CPU cores your machine has,

```sh
nproc
```

This will print a number, use that in the below command, `make` will start the build process and the number 4 represents the number of CPU cores.

```sh
make -j 4
```

Make install The default Python installation is `/usr/bin`. If you want to install Python under `/usr/local/bin` instead of overwriting the default, do this:

```sh
sudo make altinstall
```

## Verify Python 3.11 installation

```sh
python3.11 --version
```

Output:

```
Python 3.11.10
```

This concludes our topic of installing Python 3.11 version.

