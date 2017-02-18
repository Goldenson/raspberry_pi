# Media Center wiht OSMC on raspberry pi:

## What is a media center ?

> A media center is basically an interface introducing your media content.

## What do we need ?

- 1 raspberry pi
- 1 SD card (16Go)
- 1 HDMI cable
- 1 ethernet cable

## Install osmc on your SD card

Download the installer for your [os](https://osmc.tv/download/) and follow
the instructions.

Then plug in your SD card in your raspberry pi and keep following the instructions
on your TV

## Setup an enjoyable environment

First we need to find the ip address of your raspberry and connect to it through ssh. The default password for osmc will be **osmc**.
```
ssh osmc@192.168.1.69

```

**Security first:** Once you're login to your raspberry run the command `passwd` to change your current password.

Before customize our media center, we will install few things
in order to make our shell life easier.

```
sudo apt-get update && sudo apt-get install zsh && sudo apt-get install git && sudo apt-get install vim && sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

If you're seeing some undefined commands, add this line to your oh-my-zsh config file (./zshrc):
```
PATH=$HOME/bin:/usr/local/bin:/sbin:/usr/sbin:$PATH
```

Let's intall few others :shit:
`sudo apt-get install rsync && sudo apt-get install cron`

# Going deeper

In this part we will send our data to another raspberry pi to offer the same service to grandpa right.

Let's create an asynchronous replication of these data to another rasbperry.

**Go to the begining of this tuto and redo all steps.**

## Time to connect the second pi
Let's create a connection bewteen these two pi's.

```
sudo ssh-keygen -t rsa -b 2048
sudo ssh-copy-id osmc@xxx.xxx.xxx
```

Congrats!! You can now ssh to your second pi through your first one.

We will need `rsync` too, so let's install it
```
sudo apt-get install rsync
```

## Sending the data

Now we gonna create a script to send all the data to our second pi

Run tis
```
sudo rsync -avhzrPlt -stats --ignore-existing  osmc@90.127.185.136:/media/Elements/OSMC/Downloads/test/ /media/blablabla/
```

And add it to your crontab

## Workflow

1- Send file through `scp` : `scp ~/Downloads/torrent osmc@192.168.1.32:/media/Elements/OSMC/Downloads`
2- Add a torrent manually: `transmission-remote --auth=osmc:osmc -a /media/Elements/OSMC/Downloads/torrents/your_torrent`
3- Remove the first tracker: `transmission-remote --auth=osmc:osmc -l | grep \% | awk '{print $1}' | xargs -n 1 -I \% transmission-remote --auth=osmc:osmc -t \% -tr 0`
4- Remove torrent from transmission `transmission-remote --auth osmc:osmc -l | grep 100\% | grep Done | awk '{print $1}' | xargs -n 1 -I \% transmission-remote --auth osmc:osmc -t \% -r` done automatically because transmission execute the script: “/root/mvfile.sh”
