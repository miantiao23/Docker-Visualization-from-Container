
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

I used the command `gnuradio-companion` to start the app. I thought it would not work. However, it works. 
![Screenshot from 2023-10-21 11-25-35](https://github.com/miantiao23/Docker-Visualization-from-container/assets/15344076/2e929fa1-c810-4335-94bc-ee850fd2e131)


With this fact, I begin to think the dependencies in the docker image are lost. I tried to install the `xvfb`, but it took me a long time to install it. I thought it would appear in the same situation that the system needs you to configure the GUI interface. I also thought maybe the `X11` server had some issues. I reinstalled the containers `x11` server. I spent two days fixing all the missing dependencies I thought. All attempts failed. I did not see anything.

I tried to forget everything I knew before and restart to study to how use it and make it happen again. So I started a Youtube video to start it. The link is as follows: 
```
https://www.youtube.com/watch?v=L4nqky8qGm8
```
The author just pulled the latest version of `ubuntu` from `docker hub` and installed a gui-based edit app, `abiword`, to enable the interactive face. I followed his commands to creat a docker file in a specific directory. 
```
FROM ubuntu
RUN apt-get update && \
    apt-get install -y abiword
CMD abiword
```
And then, I use the `build` command to build the image. 
```
docker build -t abiword .
```
Use the same `x11` configuration for the GUI. 
```
docker run -it -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=unix$DISPLAY abiword
```
It works. 

![Screenshot from 2023-10-21 11-41-23](https://github.com/miantiao23/Docker-Visualization-from-container/assets/15344076/5b16ce76-9345-41ac-8d34-307734c7b77b)

I tried to install `sublime-text` and `vscode` to the container. Both work. In the processing, I found something about the graphic configuration installed which may be the dependencies I missed. 
I recreated a container from `ubuntu/cuda:torch1` with dockerfile in which I installed `abiword` automatically.  However, the whole process got stuck in the configuration of `geographical` and `time zone`. I had to delete the container recreate another one and install `abiword` manually with the configuration of `geographical` and `time zone`. Right now, it works as I expected. 

To start an existing container, the docker command is as follows: 
```
docker exec -it <container ID> bash
```
Update on 10/26/2023：

Just found the issues in some platforms. After you restart your host, the GUI will lose the connections to the 'DISPLAY', like the attached screenshot: 

![Screenshot from 2023-10-26 08-05-30](https://github.com/miantiao23/Docker-Visualization-from-container/assets/15344076/58e48740-b8cc-4825-bccd-51e809593c5d)

It took me almost two days to figure out what was wrong with the connection. I just accidentally retyped the command, and the connection is back: 
```
xhost +
```
My understanding right now is that the privilege of the connection was on hold before, and then I used the command `xhost +` to release the connection privilege to the docker again. 


