# Setting up a repository from the DVD

Sometimes it can be necessary to set up a local repository, with the packages from the installation DVD.

Step one is to download the ISO and mount it. We can download the ISO file with wget

```bash
wget <file link>
```

We now need to create a directory for the ISO file to be mounted to.

```bash
mkdir -p /mnt/disc

## for ISO, do this:
mount -o loop <iso file> /mnt/disc

## if we have the dvd in the drive, do this:
mount /dev/sr0 /mnt/disc
```

If mounted correctly, the system should give a warning, that /mnt/disc is mounted as read only.

We now need to create a .repo file, that tells yum/dnf where the repo files are located.

```bash
vim /etc/yum.repos.d/rhel9dvd.repo

[BaseOS]
name=BaseOS Packages Red Hat Enterprise Linux 9
metadata_expire=-1
gpgcheck=1
enabled=1
baseurl=file:///mnt/disc/BaseOS/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release

[AppStream]
name=AppStream Packages Red Hat Enterprise Linux 9
metadata_expire=-1
gpgcheck=1
enabled=1
baseurl=file:///mnt/disc/AppStream/
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-redhat-release
```

The different fields meanings:

- [BaseOS] - Repo for the packages of the base OS.

- name - The name of the repository. This should be descriptive, so we can tell it apart from other repositories 

- metada_expire - time in seconds, before the metadata expires, and needs to be redownloaded by yum/dnf

- gpgcheck - defines if yum/dnf should check for the presence of a gpgkey to verify the repository

- enabled - 1 means this repo is enabled, 0 means disabled.

- baseurl - the location of the packages. if the location is inside the filesystem, we use "file://<path to files>. example: file:///mnt/disc/BaseOS/. Note that there are 3 slashes.

- gpgkey - the location of the gpgkey. like with baseurl we should note the 3 slashes, if the repository is in the file system.



Once the repository file has been created, we need to clean the yum/dnf cache, and then we can show the repo list, to make sure our repository is shown.

```bash
yum clean all
yum repolist

Updating Subscription Management repositories.
repo id                                                         repo name
AppStream                                                       AppStream Packages Red Hat Enterprise Linux 9 (LOCALHOST)
BaseOS                                                          BaseOS Packages Red Hat Enterprise Linux 9 (LOCALHOST)
rhel-9-for-x86_64-appstream-rpms                                Red Hat Enterprise Linux 9 for x86_64 - AppStream (RPMs)
rhel-9-for-x86_64-baseos-rpms                                   Red Hat Enterprise Linux 9 for x86_64 - BaseOS (RPMs)
```

if we get no errors, we can install packages from the newly created repositories with yum/dnf.


