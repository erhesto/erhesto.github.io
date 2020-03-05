---
layout: post
title:  "Struggling with WSL"
date:   2020-03-05 23:32:02 +0100
categories: jekyll update
---

Some time ago GitHub released a beta version of Github CLI, a command-line interface for few common resources that you, as a developer, are probably using on a daily basis.
As there are detailed instructions on how to install and use it in most common operating systems like Windows, macOS or some specific Linux distros, it might be not obvious how to use it from inside WSL.
Installation for Ubuntu is pretty straightforward - all you need to do is follow the instruction:
1. Download a suitable package from the releases list: "https://github.com/cli/cli/releases/". In my case it was `gh_0.6.1_linux_amd64.deb`
```wget https://github.com/cli/cli/releases/download/v0.6.1/gh_0.6.1_linux_amd64.deb```
2. Then installation using `dpkg`:
```sudo dpkg -i gh_0.6.1_linux_amd64.deb```
3. Once this is installed, you need to access some directory with origin set to Github and try on of CLI options, like ```gh pr list```.
4. You will be asked for authentication. This can be done in several ways, but the easiest is with opening the browser and login via OAuth. This is kinda problematic from inside the terminal, as ```xdg-open``` not necessarily has an association to text-based terminal browser, and adding such not always help. Thus, the way to go might be to send the X-es directly to Windows.
5. You’ll need some X server in Windows where you can forward X to. One option that I can surely recommend in 2020 is VcXserv.
6. Once you’ve got X server installed, you might need to tweak your sshd server - some possible configuration that might work: https://askubuntu.com/questions/1157029/ubuntu-xming-on-windows-wsl-can-open-gui-from-ssh-command-but-not-during-ss.
7. The last thing - setting DISPLAY. Previously I was able to setup it with ```DISPLAY:=localhost:0.0```, but since WSL2 I needed to actually find proper IP of the host machine, so finally ended up with this script added to ```~/.zshrc```:
```export DISPLAY=$(cat /etc/resolv.conf | grep nameserver | awk '{print $2}'):0```

Bam! Now you can open firefox (or any other browser that you’ve installed in your WLS) in Windows X server and finish your authentication.


