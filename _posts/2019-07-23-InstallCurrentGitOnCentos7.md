---
layout: post
title: CentOS 7 - Install current (v 2.x) Git
description: ""
modified: 2019-07-23
tags: [CentOS, Git]
share: false
image:
  path: /images/abstract-10.jpg
  feature: abstract-10.jpg
---

Simply installing git via `yum install git` deploys an old version of git (v1.8) on your machine.
This is clearly not very usefull. For most git based applications, like Bitbucket, we need a recent version of git.  

There exist two ways of doing this: Add a new rpm repository to the system which provides a recent git version or build git from source.
Because I am not a friend of adding repositories to a system, just to simply install _one_ package, we are going to built git from source.  

You can find the current releases on [GitHub](https://github.com/git/git/releases)  

We need to first install all dependencies for the build:
```bash
sudo yum groupinstall "Development Tools" -y
sudo yum install wget perl-CPAN gettext-devel perl-devel openssl-devel zlib zlib-devel curl-devel expat-devel -y
```

The group "Development Tools" sadly contains the above mentioned old git. We need to remove it beforehand:
```bash
sudo yum remove git -y
```

Now we can download a specific version of git and extract the source files on our system:
```bash
export GITVER="2.22.0"
wget https://github.com/git/git/archive/v${GITVER}.tar.gz
tar -xvf v${GITVER}.tar.gz
rm -f v${GITVER}.tar.gz
cd git-*
```

Before we run the install process, some configuration needs to be done via `configure`
```bash
sudo make clean
sudo make configure
./configure --prefix=/usr/local/git
```

Finally the install process can be started. After installation, we tidy a bit up:
```bash
sudo make install
cd .. && sudo rm -rf git-*
sudo su -c 'echo "pathmunge /usr/local/git/bin" > /etc/profile.d/git.sh'
sudo yum group remove "Development Tools" -y
sudo yum remove perl-CPAN gettext-devel perl-devel openssl-devel zlib-devel curl-devel expat-devel -y
```
