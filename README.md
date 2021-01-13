# Ansible-OpenVPN
This is an Ansible project which is used to set up OpenVPN server on ubuntu instance. We need to provide IP address of this instance with port 22 open as ansible internally uses SSH to do the setup. In this project, we first setup OpenVPN on instance and then create client ovpn file which is downloaded locally so we can use it with VPN client tool. You need to have ansible installed on your local machine from where you are running these commands.

This is a fork of [OpenVPNAnsible](https://github.com/MiteshSharma/OpenVPNAnsible)
## Supported Targets
The following Linux distros are tested:

Ubuntu 18.04

Other distros and versions may work too. If support for another distro is desired, submit an issue ticket. Pull requests are always welcome.
## Ansible Configuration
Generate the key at the Ansible service side and copy the public key into the node.
Generate the key:
```
ssh-keygen
```
Copy to other nodes:
```
ssh-copy-id

ssh-copy-id root@xxx.xxx.xxx.xxx
```

## OpenVPN server and client setup using Ansible
This project has two ansible roles:

openVPNServer role: To create OpenVPN server setup
openVPNClient role: To create OpenVPN client ovpn file
You can change variables for openVPNClient role so you can create ovpn files with different users.

playbook.yml is main ansible file which is executed by ansible command.

Steps to execute this project:

Clone this project on your local machine

Start your ubuntu instance with port 22 open to be accessed from your local machine

Update inventory file with correct IP address of an ubuntu instance

Execute Ansible command mentioned below:

Ansible command: ```ansible-playbook playbook.yml```


## Example in Lab
Ansible-host:```server@192.168.102.4```

OpenVPN-server:```root@192.168.101.5```

Client list: client1:```test3@192.168.102.3```

### Step1:
Clone this project on Ansible-host
```
git clone ssh://git@gitlab.devtools.intel.com:29418/changku1/ansible-openvpn.git
cd ~/ansible-openvpnvim
```
revise inventory ```vim inventory```
```
[openVPN]
192.168.101.5
```

### Step2:
On Ansible-host:
```ansible-playbook playbook.yml```

### Step3:
On OpenVPN-server:
```
cd /root
cd openvpn-ca
scp xxx.ovpn client1@client1:~
service openvpn@server status
```

On OpenVPN-client1:
```
sudo apt-get update
sudo apt-get install openvpn
cp xxx.ovpn /etc/openvpn/client1.conf
sudo systemctl enable openvpn@client1.service
sudo service openvpn@client1 start
```
At this point all you need to do is wait a few seconds for the connection to complete. To check the status of the connection, use this command:
```
sudo service openvpn@client1 status
```

# Supported Target
Raspberry Pi Buster

## 更换软件更新源
```sudo vi /etc/apt/sources.list```
注释掉原内容，加上
```shell
deb http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ buster main contrib non-free rpi
deb http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ stretch main contrib non-free rpi
deb-src http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ buster main contrib non-free rpi
deb-src http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/ stretch main contrib non-free rpi
```

## 更换系统更新源
```sudo vi /etc/apt/sources.list.d/raspi.list```
注释掉原内容，加上
```shell
deb http://mirror.tuna.tsinghua.edu.cn/raspberrypi/ buster main ui
deb http://mirror.tuna.tsinghua.edu.cn/raspberrypi/ stretch main ui
deb-src http://mirror.tuna.tsinghua.edu.cn/raspberrypi/ buster main ui
deb-src http://mirror.tuna.tsinghua.edu.cn/raspberrypi/ stretch main ui
```

## 下载ufw
```shell
sudo apt update
sudo apt install ufw
```

