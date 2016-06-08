# Description 
[pd-OMXplayer](https://github.com/gllmAR/pd-omxplayer) is a [pureData](http://puredata.info) wrapper for [OMXplayer](https://github.com/popcornmix/omxplayer).
It is based on the shell extern in the [ggee](https://puredata.info/downloads/ggee) library and a modified version of [dbuscontrol.sh](https://github.com/popcornmix/omxplayer/blob/master/dbuscontrol.sh).

The core patch [omxplayer.pd](https://github.com/gllmAR/pd-omxplayer/blob/master/omxplayer.pd) instantiate OMXplayer instances and sends dbus command to each instances with this script [OMXdbuscontrol.sh](https://github.com/gllmAR/pd-omxplayer/blob/master/OMXdbuscontrol.sh). 

[omx-osc.pd](https://github.com/gllmAR/pd-omxplayer/blob/master/omx-osc.pd) and [omx-remote.pd](https://github.com/gllmAR/pd-omxplayer/blob/master/omx-remote.pd) expose OSC remote control 

[pd-OMXplayer](https://github.com/gllmAR/pd-omxplayer) as been developed raspbian-lite without X and it is meant to run this way.


# Installation

#### OMXPlayer
Remove installed omxplayer 

```
apt-get remove omxplayer

```

Compile and Install the last version of OMXPlayer

follow the instructions here

[https://github.com/popcornmix/omxplayer](https://github.com/popcornmix/omxplayer)

Quirk: first time I tried to compile ffmpeg, it didn't work, I removed OMXplayer folder and started the process again and it worked.

#### Pure Data

For the sake of simplicity, the included version in the apt-get method works fine. No need to compile here.


```
apt-get install puredata-core pd-ggee 
```

### pd-omxplayer

Create a folder for the stuff:
```
cd ~/
mkdir src
cd src
```

Clone the repositories with the dependencies  
```
git clone --recursive https://github.com/gllmAR/pd-omxplayer
```

Change permission for execution of the script OMXdbuscontrol.sh

```
cd pd-omxplayer
chmod +rx OMXdbuscontrol.sh
```

optional  : copy script to bin folder 

```
sudo cp OMXdbuscontrol.sh /usr/local/bin
```

launch the patch omx-osc.pd on the raspberry pi 

```
pd -nogui ~/src/pd-omxplayer/omx-osc.pd 
```

Send commands with omx-remote.pd (change the adress ip to address ip of your pi)

