# Quick Tips - Linux Server

This is a collection of small tips I want to remember, that does not an entire page all to themselves

## Check if a server needs a reboot

When I SSH in to an Ubuntu Server, that has automated Security updates enabled, the SSH Splash Screen will show if the server needs a reboot with this message:

```bash
*** System restart required ***
Last login: Sun Jan  9 17:44:51 2022 from 10.0.0.100
```

That is a very handy feature, but only shows up when I log in to the server, so how do I check if the server needs to be rebooted, after I have manually triggered an update?  
Well, there is a nifty little trick for that.  
If the server needs a reboot, it will create a certain file. Tthat file will be deleted upon reboot. So to check if the server needs a reboot, all I need to do, is check if this file is present.  
The file is named " reboot-required " and if present, can be found in " /var/run/ "

**Reboot Required:**

```bash
username@servername:~$ ls /var/run/reboot-required
/var/run/reboot-required
```

**Reboot not required:**

```bash
username@servername:~$ ls /var/run/reboot-required
ls: cannot access '/var/run/reboot-required': No such file or directory
```


