---
layout: post
title:  "Running .Net Core solution in Linux Mint 19/Hyper-v"
date:   2018-08-20 13:59:00 -0600
categories: C#
---

Running [CoreReact](https://github.com/chesteryang/CoreReact){:target="_blank"} in Linux Mint 19/Hyper-v

1. Install Linux Mint 19 into Hyper-v.
	- Download [Linux Mint 19](https://linuxmint.com/edition.php?id=254){:target="_blank"} iso file.
	- Create a vm on Hyper-v with the following parameters.
		- Generation 2
		- No remoteFX 3D Video Adapter
		- 2048Gb memory start, dynamic increasing
		- 40Gb disk
		- loading iso file
		- Security - Enable Secure Boot - Microsoft UEFI Certificate Authority
	- Start vm.
	- Click Install Linux Mint 19 etc. 
2. Restart Vm.
3. Modify screen resolution to 1920x1080
	- sudo nano /etc/default/grub
	- modify line GRUB_CMDLINE_LINUX_DEFAULT, to "quiet splash video=hyperv_fb:1920x1080"
	- save and exit
	- update-grub
	- reboot.
	- CTRL + ALT + Break toggle Hyper-v full screen.
4. sudo apt install git.
5. Go to [nodejs website](https://nodejs.org/en/download/package-manager/#debian-and-ubuntu-based-linux-distributions){:target="_blank"}, following instructions to install nodejs & npm.
6. Go to [dotnet core website](https://www.microsoft.com/net/download/linux-package-manager/ubuntu18-04/sdk-current){:target="_blank"}, following instructions to install dotnet core.
7. Clone CoreReact solution from github.
8. Install Visual Studio Code + C#, GitLens, TSLint, ESLint extensions.
9. Restore C# solution and npm packages.
10. Increate [watchers' handles](https://code.visualstudio.com/docs/setup/linux#_visual-studio-code-is-unable-to-watch-for-file-changes-in-this-large-workspace-error-enospc){:target="_blank"}.	
11. Start to debug.
12. Linux is case-sensitive but Windows is not. Due to this difference, when running in Linux, a defect was found: database name (Chinook.db) in config and actual file name (chinook.db) is different.