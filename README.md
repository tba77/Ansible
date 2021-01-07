# Some useful Ansible scripts I use to configure my servers

## Windows configuration

You have to first enable winrm for windows servers it's disabled by default

* Download the script https://github.com/tba77/Ansible/blob/main/ConfigureRemotingForAnsible.ps1
* Copy the script on your windows machine
* execute `c:\> powershell.exe ConfigureRemotingForAnsible.ps1 -Verbose`

## Install Ansible

I will cover only installation on linux and Mac OS as I didn't try on windows system

* Install python3-pip `apt install python3-pip` or `yum install python3 python3-pip`
* Install ansible `pip3 install ansible
* Install pywinrm version >= 0.2.2 `pip3 install "pywinrm>=0.2.2"` otherwise you'll get an error about winrm not avaiblable

## Execute scripts

1st you have to build your inventory it's a simple text file in which you fill some informations about the servers you will configure for example in my case 

```
[linux-servers]

MyLinuxServer     ansible_host=IP_ADDRESS

[windows-servers]

MyWindowsServer   ansible_host-IP_ADDRESS   ansible_password=mypassword

[windows-servers:vars]
ansible_connection=winrm
ansible_user=administrateur
ansible_winrm_server_cert_validation=ignore
```

To execute script for example I would like to extend my C drive 

`ansible-playbook -i PATH_TO_MY_INVENTORY resizeDiskScript.yaml --extra-vars "remote_host=MyWindowsServer"`

I am using extra_vars attribute because I provision a lot of servers daily so my inventory changes every day so I can add variable when executing my scripts

On linux let's add our public ssh key to avoid asking for password 

`ansible-playbook -i PATH_TO_INVENTORY addkey.yaml --extra-vars "remote_host=MyLinuxServer" --ask-pass`


