# UCK - Ubuntu Customization Toolkit
----------------------------------
## quickstart
```shell
wget https://github.com/wanlay/UCK/releases/download/v2.4.8/uck_2.4.8-0ubuntu1_all.deb
sudo dpkg -i uck_2.4.8-0ubuntu1_all.deb
```
## install from sources
```shell
git clone https://github.com/wanlay/UCK.git
cd UCK/ && make dist
# you can find the deb in the dist/ directory  
cd dist
sudo dpkg -i uck_2.4.8-0ubuntu1_all.deb
```

About the usage,reffer to the article
http://www.makeuseof.com/tag/ubuntu-customization-kit-linux-operating-system/
## What it is:
	Ubuntu Customization Kit is a tool that helps you to customize official
	Ubuntu Live CDs (including Kubuntu/Xubuntu and Edubuntu) to your needs.
	You can add (almost) any package to the live system, for example
	language packs or applications, you can add additional files to the
	CD and you can also remove packages, language packs or files that
	came with the original distribution.

## How it works:
	UCK works by extracting the contents of the Live CD to your disk,
	unpacking the root file system stored on the CD in the process.
	Customization is then performed either on the level of the ISO
	image or on the level of the root file system extracted. The resulting
	root file system is then repacked and intergrated back into a new
	ISO image that can be burned to CD/DVD creating a customized Live
	CD/DVD or copied onto an USB stick with usbcreator.

## Prerequisites:
	You will need a fairly recent working Ubuntu installation of the same
	architecture as the Live CD image you want to customize (cf.
	Restrictions below) on either a virtual machine or a workstation with
	a sufficently large disk.

	Although it may be possible to run UCK on other distributions, this
	is not a supported setup.

## Limitations and Caveats:
	1. UCK requires root privileges for many of the steps. You need to be
	  able to run sudo (fakeroot will not suffice!) on the machine that you
	  want to perform customization on. It is generally not a good idea to
	  run UCK on a server that must be available for production purposes.
	  It's way better to use a private workstation with no valuable data or
	  a virtual machine as host.

> You have been warned!

	2. UCK performs customization of the root file system by running commands
	  (apt-get install et.al.) as root in a chroot environment. This implies
	  that:

	  1.  You generally cannot customize Live CD images for machine
	     architectures different from the one you run UCK on, e.g.
	     customizing an image for ARM processors will not be possible
	     if your machine is x86 based. One exception to this rule is that
	     you can customize x86 images on an x86_64 host because 32bit
	     binaries can be run on 64bit Linux systems - the reverse (i.e.
	     customizing a 64bit system on a 32bit host) is not possible.

	  2.  Some commands may not work in the chroot environment, especially
	     if they require services that can only be run once on a system.

	  3.  While installing/removing packages in the chroot environment,
	     services may be stopped/started by the packaging scripts. You will
	     need to take care that these services (a) do not collide with
	     services running on the customization host and (b) that they
	     get terminated near the end of the root file system customization
	     because otherwise it may be impossible to successfully finish
	     the customization.

	  4.  Even though commands run in a chroot environment they run with
	     full root privileges. It is therefore possible that the hosting
	     system is corrupted by commands going astray, e.g. if the date
	     command is run to set the date/time in the chrooted environment
	     the date/time of the hosting environment will be affected. Even
	     worse: Should a command create a device node that represents a
	     device in the hosting system and then decide to format that device
	     (or to install a bootloader someplace) the hosting system will
	     be unusable afterwards.
	     
	  5.  Before entering the chroot environment an "xhost +" command is
	     called to enable remote access to X, this won't be reset at the
	     end of the process (because we can't know what were the previous
	     settings).

	  6.  If you update grub in your chroot environment you can have troubles,
	     every software that dialogs with hardware (while installing) could
	     can couse troblues and UCK cannot detect or work around this thing.
	     grub-probe binary is deactivated (renamed to grub-probe.uck-blocked
	     and symlinked to /bin/true) so if you update grub you'll have to take
	     care of this, renaming grub-probe to grub-probe.uck-blocked before
	     leaving the chrooted environment.
	     
> Again: You have been warned!

