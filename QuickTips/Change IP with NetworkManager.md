RHEL servers use NetworkManager by default to manage networking connections. 
On ubuntu servers, with netplan, the networking renderer can be set to use NetworkManager aswell.

### See Network Devices

To see the servers available network devices, we can use the nmcli command
```bash
nmcli device status
DEVICE  TYPE      STATE                   CONNECTION 
ens18   ethernet  connected               static     
ens19   ethernet  connected               rhcsa      
lo      loopback  connected (externally)  lo         
```

in this case, we will change the IP adress of the ens19 interface.

in RHEL 8 and older, the configuration files for the interfaces configured with networkmanager where called ifcfg files and where stored in /etc/sysconfig/network-scripts/.
This is no longer the case from RHEL9 and going forward. (though is still supported for the time being...). Instead key files are used. These are by default stored in the /etc/NetworkManager/system-connections/ folder.

example:
```bash
sudo cat /etc/NetworkManager/system-connections/rhcsa.nmconnection
[connection]
id=rhcsa
uuid=25e8e47d-4f4d-4b1a-8936-dc91c90b4c54
type=ethernet
interface-name=ens19
timestamp=1681413161

[ethernet]

[ipv4]
address1=192.168.1.12/24
method=manual

[ipv6]
addr-gen-mode=default
method=auto

[proxy]

```

### nmtui and nmcli

To change the settings of the network connection, we can either use the TUI based program, called " nmtui " or the cli based option, called " nmcli ". Neither is more correct than the other. Though for this guide we are going to focus on nmcli.

nmtui looks like this:
![[nmtui.png]]

### nmcli options

- con-name - name of the network connection
- type - is this ethernet, wifi, or something completely different.
- ifname - the name of the network interface we wish to use for this connection
- autoconnect - yes or no. Should this connection be used by automatically. Remember to make sure no other connections on the same interface uses autoconnect.
- ipv4.method - auto for DHCP, manual for static
- ipv4.addresses - type the desired IP adress here
- ipv4.gateway - IP address of the default gateway
- ipv4.dns - IP address of of the DNS servers. to set two add the ip adresses in a set of quotation marks, with a space between. Like so: "8.8.8.8 1.1.1.1"

### The actual command

the command:
```bash
nmcli connection add con-name rhcsa2 type ethernet ifname ens19 autoconnect yes ipv4.method manual ipv4.addresses 192.168.1.5/24 ipv4.gateway 192.168.1.1 ipv4.dns "192.168.1.1 8.8.8.8"
Connection 'rhcsa2' (2c1c26a1-dbdb-481c-bf11-63c2ad62aab6) successfully added.
```

### Bring the connection up

once the connection has been added, it needs to be started.
we do this with the nmcli con up [name].

example:
```bash
nmcli con up rhcsa2
Connection successfully activated (D-Bus active path: /org/freedesktop/NetworkManager/ActiveConnection/4)
```

To see which connections are available, and which are up, we can use the nmcli con show command.

```bash
nmcli con show
NAME                UUID                                  TYPE      DEVICE 
static              c93aad00-434a-4f45-b1f6-01082d7e9d84  ethernet  ens18  
lo                  06516760-073d-4d52-8286-41cb37a708d2  loopback  lo     
rhcsa2              2c1c26a1-dbdb-481c-bf11-63c2ad62aab6  ethernet  ens19  
ens18               93771c70-712b-3127-b961-3e992507fb8c  ethernet  --     
rhcsa               25e8e47d-4f4d-4b1a-8936-dc91c90b4c54  ethernet  --     
Wired connection 1  3c74e68b-e2a3-3fd3-9e15-28565e64cde4  ethernet  --     
```


### Add an extra IP address

it is possible to add extra ip addresses after the fact, with nmcli (and with nmtui).

To add extra options (IP adresses, DNS adress, and so on), with the command "nmcli con mod"

The command:
```bash
nmcli con mod rhcsa +ipv4.address 192.168.1.100/24
```

to remove the IP adress again, simply use " -ipv4.address " instead of a +

We can ofcourse also add ipv6 addresses like this:

```bash
nmcli con mod rhcsa +ipv6.address fd01::105/64
```