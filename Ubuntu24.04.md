# Install for ubuntu 24.04 KDE

## 1. install dlib by source
```bash
# install lib
sudo apt-get update
sudo apt-get install build-essential cmake libgtk-3-dev libboost-all-dev
sudo apt-get update && sudo apt-get install -y python3 python3-pip python3-setuptools python3-wheel cmake make build-essential libpam0g-dev libinih-dev libevdev-dev python3-dev libopencv-dev

# clone
git clone https://github.com/davisking/dlib.git
cd dlib

python3.11 -m venv venv
python3.11 -m venv venv

./venv/bin/python -m build --wheel
./venv/bin/python -m build --wheel
# install dlib which buit:
./venv/bin/pip install dist/dlib-19.24.99-cp312-cp312-linux_x86_64.whl

# test module
sudo python3.11 show dlib

```

## 2. install howdy
> ignore error, just install by the way

```bash
sudo apt install howdy
# install some lib that lost when `apt install howdy` error before this step:
python3.11 -m pip install numpy opencv-python-headless opencv-python ConfigParser
```


### config:
```bash
sudo cp howdy.sh /lib/security/howdy/howdy
chmod +x /lib/security/howdy/howdy
sudo ln /lib/security/howdy/howdy /usr/local/bin/howdy
sudo cp -f compare.py /lib/security/howdy/
sudo cp -f pam.py /lib/security/howdy/
## edit /lib/security/howdy/config.ini for video device and something for work

```
### edit /etc/pam.d
```bash
cat /etc/pam.d/sudo
#%PAM-1.0

# Set up user limits from /etc/security/limits.conf.
### for howdy face detection
auth   sufficient        pam_python.so /lib/security/howdy/pam.py
```

```bash
cat /etc/pam.d/sddm | head -n 5
#%PAM-1.0

# added howdy
auth sufficient pam_python.so /lib/security/howdy/pam.py

```

```bash
cat /etc/pam.d/common-auth
## for howdy face detection
auth       [success=1 default=ignore] pam_python.so /lib/security/howdy/pam.py
```

## Testing
```bash
# add face
sudo howdy add
# test video detect
sudo howdy test
# test login
sudo <some command >
```