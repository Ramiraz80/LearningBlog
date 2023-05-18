# Autotiling in i3

Link: https://github.com/nwg-piotr/autotiling

Download the main.py file from the autotiling folder.

Install python3-i3ipc

```bash
sudo apt install python3-i3ipc
```

Move the main.py file from Downloads to /usr/bin and rename it autotiling

```bash
sudo mv main.py /usr/bin/autotiling
```

Make the autotiling file executable

```bash
sudo chmod +x /usr/bin/autotiling
```

add the autotiling to i3 config

```bash
vim ~/.config/i3/config
### Add the following two lines

# enable autotiling (from https://github.com/nwg-piotr/autotiling )
exec_always --no-startup-id autotiling
```
