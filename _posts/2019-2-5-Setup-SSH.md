---
layout: post
title: Setup SSH Keys on Linux Systems
image: https://user-images.githubusercontent.com/18272237/52320647-969db400-299e-11e9-8f1e-eaf581275d38.jpg
---

I am using this because it provides a secure way of accessing a server via SSH.

## Procedure:
1. Create key pair using ssh-keygen command
2. Copy and install the public key using ssh-copy-id command
3. Test changes
4. Disable the password login for root account

##### Key locations:
```bash
~/.ssh/id_rsa # Private Key
~/.ssh/id_rsa.pub # Public Key
```

## 1: Create Key Pair
```bash
# Run on client machine
ssh-keygen -t rsa -b 4096 -f ~/.ssh/client.key -C "The VPS Server key"
```

## 2: Install public key on remote server
##### Automatic: 
Use scp or ssh-copy-id command to copy your clients public key file to the remote system
```bash
# Run on remote system(VPS)
ssh-copy-id -i ~/.ssh/client.pub <user>@<Server IP/Hostname>
scp ~/.ssh/client.pub <user>@<Server IP/Hostname>:~/.ssh/authorized_keys
```
##### Manual: 
Copy your client's public key from ~/.ssh/client.pub to remote systems authroized_keys directory ~/.ssh/authorized_keys

## 3: Test it out
```bash
ssh <user>@<Server IP/Hostname>
# You will be prompted for the password of the private key.
# To get rid of asking for the passphrase whenever you login, try ssh-agent or ssh-add, follow below:
eval $(ssh-agent)
ssh-add
# Now try to see if it asks for a passphrase:
ssh <user>@<Server IP/Hostname>
```

## 4: Disable password login via SSH
```bash
# Login to the remote system
ssh <user>@<Server IP/Hostname>
# Edit ssh config file
sudo vim /etc/ssh/sshd_config

# Jump to PermitRootLogin
sudo vim +/PermitRootLogin /etc/ssh/sshd_config
```
Find PermitRootLogin and set to no:
```bash
PermitRootLogin no
```
Save and close the file

##### Reload SSHD Server:
```bash
sudo systemctl reload sshd
# or
sudo systemctl reload ssh
```


## Extras
##### Add or Replace passphrase for existing private key
```bash
ssh-keygen -p
```

_**It is always a good idea to backup your keys..**_


***
[Reference](https://www.cyberciti.biz/faq/how-to-set-up-ssh-keys-on-linux-unix/)
