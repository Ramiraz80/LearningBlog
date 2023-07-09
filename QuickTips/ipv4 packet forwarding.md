If we are setting up a server as a firewall, router, or NAT device, we will need packet forwarding.
Remember it should only be turned on, in these instances.

### Check if ipv4 packet forwarding is enabled

On most systems, we can use the sysctl command to check the status of packet forwarding.
```bash
[root@rhcsa2 ~] sysctl net.ipv4.ip_forward
net.ipv4.ip_forward = 0
```
the 0 means that is is turned off at the moment. It would be a 1, if turned on.

Another way to see this, is to look inside the /proc/sys/net/ipv4/ip_forward file. Here again 0 means disabled.
```bash
[root@rhcsa2 ~] cat /proc/sys/net/ipv4/ip_forward
0
```

### Non persistant change

If we just need the feature enabled temporarily, we can change it with the sysctl command, with the -w flag
like so:
```bash
[root@rhcsa2 ~] sysctl -w net.ipv4.ip_forward=1
net.ipv4.ip_forward = 1
```

Alternatively we can change the number in the ip_forward file in /proc.
```bash
[root@rhcsa2 ~] echo 0 > /proc/sys/net/ipv4/ip_forward
```

As mentioned, this change does not persist across reboots.

### Persistant change

If we need to enable ipv4 packet forwarding persistently, we can edit the /etc/sysctl.conf file.
This is a dropin file, for sysctl, where we can add custom changes we want to have persistant.

```bash
[root@rhcsa2 ~] vim /etc/sysctl.conf

# sysctl settings are defined through files in
# /usr/lib/sysctl.d/, /run/sysctl.d/, and /etc/sysctl.d/.
#
# Vendors settings live in /usr/lib/sysctl.d/.
# To override a whole file, create a new file with the same in
# /etc/sysctl.d/ and put new settings there. To override
# only specific settings, add a file with a lexically later
# name in /etc/sysctl.d/ and put new settings there.
#
# For more information, see sysctl.conf(5) and sysctl.d(5).

net.ipv4.ip_forward = 1

```

To make this cjamge tale effect, either reboot the server, or use the sysctl command with the -p flag
```bash
[root@rhcsa2 ~] sysctl -p
net.ipv4.ip_forward = 1
```
