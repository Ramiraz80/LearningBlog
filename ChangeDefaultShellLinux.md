# How To: Change the default shell in Linux

To see what shell we currently have set as the default shell, we can use the “echo $SHELL” command, like so:

```bash
[ramiraz@Thor ~]$ echo $SHELL
/bin/zsh
```

If the shell listed, isn’t the one we want as default, we can change it.

We change the default shell with the command “chsh”.  
First we use it to list the available shells on the system. We do this by passing the “-l” option to the command.

```bash
[ramiraz@Thor ~]$ sudo chsh -l
/bin/sh
/bin/bash
/bin/zsh
/usr/bin/zsh
/usr/bin/git-shell
/usr/bin/fish
/bin/fish
```

For this example we want to change the default shell from ZSH to BASH.  
To do this we use the “chsh” command again, this time with the option of “-s” and with the argument of the absolute path to bash ( /bin/bash).

```bash
[ramiraz@Thor ~]$ chsh -s /bin/bash
Changing shell for ramiraz.
Password: 
Shell changed.
```

The last step to make this take effect, is to log out and back in again.

More info can be found on this Archwiki page: [Command-line shell - ArchWiki](https://wiki.archlinux.org/title/Command-line_shell#Changing_your_default_shell)


