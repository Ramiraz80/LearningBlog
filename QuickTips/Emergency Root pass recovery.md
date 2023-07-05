DEBIAN / UBUNTU root PASSWORD RECOVERY













1. First screen - grub - press e
2. Modify kernel line: add `single` between `ro quiet` and at the end of this line add `init=/bin/bash`
3. Press F10
4. When the prompt is `root@(none):/#` you have to remount the / partition to have read-write access: `mount / -rw -o remount`
5. Now you are ready to modify the root password: type `passwd` and change it!

Good luck