
Here are some experiences I have these days with the docker container. I was trying to use a container to visualize the apps from the docker containers. 
However, it did not work. I made it happen before. I checked all my history before, and I just have some docker commands left. Therefore, I got stuck for 
the commands for a long while until today. 


Here is the old docker command I left before： 
```
docker run -it --gpus all -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=unix$DISPLAY ubuntu/cuda:torch1
```
I doubted the configuration of the computer, and then I tried another container with the command: 
```
docker run -t -i --privileged -v /dev/bus/usb:/dev/bus/usb -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=unix$DISPLAY -e HOME=/home/bladerf -u bladerf ncsuarc/bladerf bash -l
```

I thought it would not work. However, it works. 



With this fact, I begin to think the dependencies in the docker image are lost. I tried to install the `xvfb`, but it took me a long time to install it. I thought it would appear in the same situation that the system needs you to configure the `GUI` interface. I also thought maybe the `X11` server had some issues. I reinstalled the containers `x11` server. I spent two days fixing all the missing dependencies I thought. All attempts failed. I did not see anything. 

