# jupyter_OfflineServer
Reminder for me on how to setup jupyter notebook on offline server

## Preparing an offline single node jupyter with no sudo access
##### Outline:
###### 1. Installing portable python
###### 2. Updating bash_profile file
###### 3. Installing python packages and its dependencies
###### 4. Running Jupyter
---
### 1. Installing portable python
* On online server, download the right python version on https://www.activestate.com/products/activepython/downloads/
I’m using python 3.6 Linux (x86_64).
* Move the file to offline server
* Extract it by `tar -xzvf ActivePython-3.6.0.3600-linux-x86_64-glibc-2.3.6-401834.tar.gz`
* Move to extract folder `cd ActivePython-3.6.0.3600-linux-x86_64-glibc-2.3.6-401834`
* Run the script `sh install.sh`
* It will ask where you want the installed folder reside. I put mine in /home/ghilmanfat/python/python36
---
### 2. Updating bash_profile file
In order for the terminal to recognize our python version, we need to update .bash_profile
*	`nano .bash_profile`
*	add the following
```
PYTHON_PATH=/home/ghilmanfat/python/python36/bin
PATH=$PATH:$PYTHON_PATH

export PATH
export ARCHFLAGS=-Wno-error=unused-command-line-argument-hard-error-in-future
alias python='$PYTHON_PATH/python3.6'
alias pip='$PYTHON_PATH/pip3.6'
```
* refresh by `source ./bash_profile`
---
### 3. Installing python packages with its dependencies
Copy paste from: https://stackoverflow.com/questions/11091623/python-packages-offline-installation
On online machine select a directory with terminal cd and run this code:
```
python -m virtualenv myenv
cd myenv
source bin/activate
pip install numpy
```
after installing all the packages, you have to generate a requirements.txt so while your virtualenv is active, write

```
pip freeze > requirements.txt
mkdir transferred_packages
mv requirements.txt transferred_packages
cd transferred_packages
```
Download the packages:

```
pip download -r requirements.txt
cd ..
tar -zcvf offline.tar.gz /transferred_packages/
```

take your offline folder to offline server and then
```
tar -xzvf offline.tar.gz
cd offline
pip install --no-index --find-links="./tranferred_packages" -r requirements.txt
```
what is in the folder offline (requirements.txt , tranferred_packages {Flask-0.10.1.tar.gz, ...})
check list of your package `pip list`

---
### 4. Running Jupyter
* Update .bashrc
```
alias jupyter=’/home/ghilmanfat/python/python36/bin/jupyter’
unset XDG_RUNTIME_DIR
```
* Restart the bashrc file by: `source .bashrc`
* Generate notebook config file: `jupyter notebook --generate-config`
* Run notebook by: `jupyter notebook --ip=0.0.0.0 --port=[any open port you have]`
* Open the ip address of your offline server with additional port and token given.
