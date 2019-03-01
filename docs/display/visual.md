# Visual Display (Optional)

When we talk about a visual display we talk about giving you the understand of what is happening in the simulator running in the cloud. 

The best example is the manual control of the simulator in the cloud. 

* Note that this is a feature of the service that will be enhanced with a more streamlined experience soon.

For a visual display of the could simulator, you will need a local X11 installation. 

* Warning: There are definite security risks involved in allowing network connections to X11 on any system. Future iterations of the service will not require X11.

## Configuration on Linux

X11 is installed on linux by default

## Configuration on OSX

Install XQuartz

```
export DISPLAY=:0
defaults write org.macosforge.xquartz.X11 nolisten_tcp -int 0
```