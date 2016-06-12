# Description 
[pd-OMXplayer](https://github.com/gllmAR/pd-omxplayer) is a [pureData](http://puredata.info) wrapper for [OMXplayer](https://github.com/popcornmix/omxplayer).
It is based on the shell extern in the [ggee](https://puredata.info/downloads/ggee) library and a modified version of [dbuscontrol.sh](https://github.com/popcornmix/omxplayer/blob/master/dbuscontrol.sh).



The core patch [omxplayer.pd](https://github.com/gllmAR/pd-omxplayer/blob/master/omxplayer.pd) instantiate OMXplayer instances and sends dbus command to each instances with this script [OMXdbuscontrol.sh](https://github.com/gllmAR/pd-omxplayer/blob/master/OMXdbuscontrol.sh). 


[omx-deamon.pd](https://github.com/gllmAR/pd-omxplayer/blob/master/omx-deamon.pd) and [omx-remote.pd](https://github.com/gllmAR/pd-omxplayer/blob/master/omx-remote.pd) expose OSC remote control paired with a deamon running on the Pi for up to 5 files with alpha blending.

[pd-OMXplayer](https://github.com/gllmAR/pd-omxplayer) has been developed raspbian-lite without X and it is meant to run this way (not tested with x11).


# Installation

## OMXPlayer

Grab a fresh compiled version of omxplayer  

At time of writing, the stock omxplayer available in raspbian is not fully compliant with all dbus controls   

Remove installed omxplayer 

```
apt-get remove omxplayer
```

Compile and Install the last version of OMXPlayer

follow the instructions here

[https://github.com/popcornmix/omxplayer](https://github.com/popcornmix/omxplayer)

Quirk: first time I tried to compile ffmpeg, it didn't work, I removed OMXplayer folder and started the process again and it worked.

## Pure Data

For the sake of simplicity, the included version in the apt-get method works fine. No need to compile here.

```
apt-get install puredata-core pd-ggee 
```

## pd-omxplayer

Create a folder src in the home directory:
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

launch omx-deamon.pd on the raspberry pi

```
pd -nogui ~/src/pd-omxplayer/omx-deamon.pd 
```

optional : copy the service deamon in the systemd service folder and activate on boot 
```
sudo cp ~/src/pd-omxplayer/service/* /etc/systemd/system/
sudo systemctl daemon-reload
sudo systemctl start pd-omx.service
sudo systemctl enable pd-omx.service
```

optional : for scrolling posting of the service: 
```
sudo journalctl -f -u pd-omx
```

# Usage
## [OMXdbuscontrol.sh](https://github.com/gllmAR/pd-omxplayer/blob/master/OMXdbuscontrol.sh)
This bash script can be used directly with OMXplayer.

Launch a omxplayer instance with a dbus name like so (where ~/media/test.mov is your media file)

```
omxplayer --dbus_name org.mpris.MediaPlayer2.omxplayer --layer 1 --blank ~/media/test.mov
```

use the script by providing the dbus name of the target and the parameters 
```
./OMXdbuscontrol.sh [dbusname] [parameter] [value1] [value2] [value3] [value4]
```

this is an example

```
./OMXdbuscontrol.sh org.mpris.MediaPlayer2.omxplayer setalpha 54
```
when the permissions are not set to execution, you can prepend `bash` to the script line

```
bash OMXdbuscontrol.sh org.mpris.MediaPlayer2.omxplayer setalpha 54
```

## [omx-deamon.pd](https://github.com/gllmAR/pd-omxplayer/blob/master/omx-deamon.pd)
omx-deamon stacks 5 omxplayers instances and bridge the shell script with OSC commands. 
It can be run at boot with the systemd service pd-omx.service



## omx-remote
Send commands with omx-remote.pd 

* Change the address IP to address IP of your pi
* Change the medias path to some actual media files 



# Addendum

## Supported features of omx-deamon

* Support multi-layers
* 5 players name /p1 to /p5

* todo
	* fix the error message when movie is manually stopped and started again, there is a little quirk.
	* add a debug flag for proper debug printing
	* get playhead position and broadcast to OSC
	* fix setposition in the script 
	
* to be fix
	* rate is not preserved after looping ()
	* 
	 


## Supported features of OMXdbuscontrol.sh

* volume [x]
* rate [x]
* setvideopos [x1 y1 x2 y2]
* setvideocroppos [x1 y1 x2 y2]
* setaspectmode [letterbox,fill,stretch,default] -todo (not working yet in pd)
* setalpha [0..255]
* seek [x]
* setposition [x] -todo (only reset  to zero)
* rateup
* ratedown
* pause
* stop
* hidevideo
* unhidevideo
* volumeup
* volumedown
* togglesubtitles
* hidesubtitles
* showsubtitles


