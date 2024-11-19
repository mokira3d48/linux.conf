On ubuntu 20.04, It's the following process  :


1. Update de system packages:
```bash
sudo apt-get update
sudo apt-get upgrade
```

2. Install dependences required :
```bash
sudo apt-get install -y software-properties-common
sudo add-apt-repository ppa:deadsnakes/ppa -y
sudo apt-get update
```

3. Install Python 3.10 and its dependences:
```bash
sudo apt-get install -y python3.10 python3.10-dev python3.10-venv python3.10-distutils liblzma-dev
```

4. Download and install pip for Python 3.10:
```bash
wget https://bootstrap.pypa.io/get-pip.py
python3.10 get-pip.py
```

