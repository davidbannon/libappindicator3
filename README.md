
===========



**Introduction**
--------


This repo contains a single library deb that may be useful for Debian Bullseye using Gnome. It provides the missing libappindicator3-1 and, incidentally, it will also pull in, from the normal Bullseye repo, gnome-shell-extension-appindicator.



It may also be of help, but has not been tested with some other Debian derived distributions. Its not needed with Ubuntu or Fedora.

 

"Why ?" - If you want to use a traditional **SysTray** (such as tomboy-ng) app on Debian Gnome Bullseye, well, bad luck, the only way to get that SysTray Icon visible is to use Ubuntu's gnome-shell-extension-appindicator plugin. And that plugin needs libappindicator3-1 which, early in 2021, Debian decided to drop as they had no maintainer for it. The plugin is still there but not the library.



However, it is possible to install libappindicator3-1 from the deb available here, this deb has an added dependency on gnome-shell-extension-appindicator, reducing the install steps just a little.



Note I have made absolutly no changes to the library code, just played with the packaging.



**Installation**
--------


Download the library deb file from release directory, install it with something like -



`sudo apt install ./libappindicator3-1_20.04-bullseye_amd64.deb`



And then logout and backing again (to restart the desktop) and then enable the plugin, doing one of 

* If you use tomboy-ng, it will offer to 'enable' the plugin when it next you start tomboy-ng up. Click 'yes'.

* Use gnome-tweaks, a GUI app, "extensions", "Ubuntu appindicators"

* Use the gnome-extensions command.  



You will now find apps like https://github.com/tomboy-notes/tomboy-ng will now show you a nice convenient SysTray Icon.



**Other Information**
--------
This library is built from unaltered source from the Ubuntu 20.04 version of libappindicator3-1. The only change I have made is to add gnome-shell-extension-appindicator to save one install step. If you feel uncomfortable installing something like this from the Interweb thing (and you should be), you can easily build it yourself (instructions below) or even just copy the library binary and associated symlink from a Ubuntu 20.04 install.



This approach is not needed on Ubuntu, everything needed is already installed "out of the box". Fedora also has the necessary packages in its repository, try this command -



`sudo dnf install libappindicator-gtk3  gnome-shell-extension-appindicator gnome-tweaks [enter]`



Then logout and back in again to restart the desktop, then enable the plugin. Unfortunately, the plugin changes its name during this process, in gnome-tweaks (Fedora 33) its under "Extensions",  "Kstatusnotifieritem/appindicator support". In Fedora 34, use this command -



`gnome-extensions enable appindicatorsupport@rgcjonas.gmail.com`



Please address your complains about this to RedHat, not me !



**Building This Library**
--------
This instructions are really my notes, so I remember how its done, you probably can stop reading now.  But if you want to build your own, go right ahead !



First, download the three deb src files from a know good libappindicator3, initially, I used the Ubuntu 20.04 one and its passed my tests so far.



`sudo apt-get source libappindicator3-1 [enter]`



Then, take the three files, *.gz, *.dsc and *.gz to the Debian Distro you want to use, say, bullseye. Those files contain all you need to make a working source tree for the library (plus a heap of other libraries incidentally)  and install devscripts there -



`sudo apt install devscripts [enter]`



In the directory with the three files, run `dpkg-source -x *.dsc`  That will build a directory containing everything needed to build those libraries. So, cd into the new directory, run dpkg-buildpackage -us -uc 



You will almost certainly get a list of packages, build dependencies, are missing. So, install them. And try `dpkg-buildpackage -us -uc` again, might need a few cycles. 



Once you can build reliably, time to make a small change to the control file, in the libappindicator3-1 section, the only section we are interested in, add a line after the the "${misc:Depends}," line that looks like -

          `gnome-shell-extension-appindicator (>=34),`



That line will add the necessary gnome plugin as a dependency of libappindicator3, its not much use without it.



Now, build the debs again, when finished, grab one that contains libappindicator3-1 and perhaps simplify its name a little. I mention 20.04 and bullseye in its name to recognise where it has come from and where its going.



**Offered in the hope its useful, there are, and cannot be, any responsibility accepted nor guarantees it will solve all your problems !**
