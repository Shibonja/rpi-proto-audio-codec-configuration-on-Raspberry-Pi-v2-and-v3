sudo nano /boot/config.txt
##########################
#uncomment lines:
dtparam=i2c_arm=on
dtparam=i2s=on
dtparam=spi=on
#add lines:
dtoverlay=i2s-mmap
dtoverlay=rpi-proto
 
sudo nano /etc/modules
######################
#add lines:
i2c-bcm2708
i2c-dev
  
  
snd_soc_bcm2708_i2s
bcm2708_dmaengine
snd_soc_wm8731
snd_soc_rpi_proto
 
#List amixer controls
amixer controls
  
numid=2,iface=MIXER,name='Master Playback ZC Switch'
numid=1,iface=MIXER,name='Master Playback Volume'
numid=4,iface=MIXER,name='Line Capture Switch'
numid=5,iface=MIXER,name='Mic Boost Volume'
numid=6,iface=MIXER,name='Mic Capture Switch'
numid=8,iface=MIXER,name='ADC High Pass Filter Switch'
numid=3,iface=MIXER,name='Capture Volume'
numid=10,iface=MIXER,name='Playback Deemphasis Switch'
numid=11,iface=MIXER,name='Input Mux'
numid=14,iface=MIXER,name='Output Mixer HiFi Playback Switch'
numid=12,iface=MIXER,name='Output Mixer Line Bypass Switch'
numid=13,iface=MIXER,name='Output Mixer Mic Sidetone Switch'
numid=7,iface=MIXER,name='Sidetone Playback Volume'
numid=9,iface=MIXER,name='Store DC Offset Switch'
  
#change control name='Output Mixer HiFi Playback Switch'
amixer cset numid=14 on
#check changes
amixer cget numid=14
numid=14,iface=MIXER,name='Output Mixer HiFi Playback Switch'
  ; type=BOOLEAN,access=rw------,values=1
  : values=on
  
#change control name=name='Mic Capture Switch'
amixer cset numid=6 on
#check changes
amixer cget numid=6
numid=6,iface=MIXER,name='Mic Capture Switch'
  ; type=BOOLEAN,access=rw------,values=1
  : values=on
  
#change control name=name='Input Mux'
amixer cset numid=11 1
#check changes
amixer cget numid=11
numid=11,iface=MIXER,name='Input Mux'
  ; type=ENUMERATED,access=rw------,values=1,items=2
  ; Item #0 'Line In'
  ; Item #1 'Mic'
  : values=1
  
#save changes
sudo alsactl store
  
sudo reboot
  
#record something
arecord -c 2 -D hw:0,0 -f S16_LE -r48000 save.wav
 
sudo nano /etc/asound.conf
###################################
pcm.!default {
    type hw
    card 0
}
ctl.!default {
    type hw
    card 0
}
###################################