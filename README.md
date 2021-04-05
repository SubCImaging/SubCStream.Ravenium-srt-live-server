# Introduction
The SubC Streaming Service has changed and was at one time using RTMP for its encoder. Then it changed to using WebRTC for streaming and now we have WebRTC and SRT streaming encoders in the program.

We also now refer to as “Offshore Real-Time Streaming” with it being build into DVRO v7 and Fugro’s custom streaming application (formerly stand-alone application).

# What is SRT?
You can find official information at [https://www.haivision.com/products/srt-secure-reliable-transport/](https://www.haivision.com/products/srt-secure-reliable-transport/ "https://www.haivision.com/products/srt-secure-reliable-transport/"), but SRT(Secure Reliable Transport) is an open source video streaming protocol that brings pristine quality, low-latency live video over the public internet.

Currently usage and projects using this technology is more towards commercial broadcasting but it is starting to make it’s way into the main stream.

# Detailed Server Information
This is a port of the Ravenium-srt-live server (https://github.com/ravenium/srt-live-server). It is a dockerization of the below packages
 - Edward Wu's SRT Live Server (SLS) - https://github.com/Edward-Wu/srt-live-server
 - Haivision's SRT SDK - https://github.com/Haivision/srt

If you need to research into any reported issues, checkout https://github.com/Edward-Wu/srt-live-server/issues for possible matches.

I have forked it manually, https://github.com/SubCImaging/SubCStream.Ravenium-srt-live-server.

## Checking the Server Status
**Steps**
1.  SSH into the server
2.  Type “sudo docker ps | grep ravenium-srt”

The resulting output should indicate a uptime. But if nothing appears from the typed command then the application has crashed or was not running for some other reason.

## Starting the Server

You can start the server by typing the command below.

`1sudo docker run -d -p 1935:1935/udp --name ravenium-srt subc/ravenium-srt:develop`

After it has been started, ensure it has been started.

## Manually Stopping and Starting the Server

If you just want to restart the server, you can do so by running the commands below.

`1sudo docker stop ravenium-srt 2sudo docker start ravenium-srt`

You can change the name of the docker container and stop and start an existing container this way as well.

As always, after starting the container, check if it is started and running.

## Deployment

Manually deployment to a new server is made possible using the command below.

`1sudo docker run -d -p 1935:1935/udp --name ravenium-srt subc/ravenium-srt:develop`

The run command will check if the image is already present and if not, download it from the SubC docker repo.

If you get into an situation when an older version of the server already deployed to the server, then you will need to pull down the new changes and run the container using the above command.

An example of the pull command is below.

`1sudo docker pull subc/ravenium-srt:develop`

# Testing

Testing can be done using the SubC Streaming system. But if you need to do more basic/manual testing you will need:

-   Visual Studio
    
-   [![](https://github.githubassets.com/favicon.ico)https://github.com/SubCImaging/SubCWriter - Restricted link,  try another account](https://github.com/SubCImaging/SubCWriter) ​applicatio
    
-   VLC 4.0 or OBS Studio

The flow for low level testing is to use the SubC Writer application to stream out content to the SRT server and then preform playback using VLC 4.0/OBS Studio

## Creating the Sending and Receiving URL’s

![](blob:https://subcimaging.atlassian.net/374b1c5d-00d2-4ef2-b19d-7655549025aa#media-blob-url=true&id=6ac7ecef-c038-418a-883d-0c51c1e739ed&collection=contentId-1283129438&contextId=1283129438&mimeType=image%2Fpng&name=image-20210405-012709.png&size=33208&width=539&height=211)

The SRT server has two sides available to it, but all working on the port 1935.

As shown above, your input URL would have the input typed and the playing unit would have the output typed.

### Sending URL

`1srt://your.server.ip:1935?streamid=input/live/yourstreamname`

### Playback URL

`1srt://your.server.ip:1935?streamid=input/live/yourstreamname`

## Setting up Streamer

To start the SubCWriter application currently, you will need to install Visual Studio and run it from there. We do not have a compiled version currently posted but that could change down the road.

Keep in mind this is a modified version of a MediaLooks sample application but often will have issues/bugs.

You may need to download and install the MediaLooks SDK to run this application as well.

Once you have everything needed to run the application, follow the steps below.

**Steps**
1.  Choose SRT from the encoder drop-down
2.  Choose h.264 or h.265 as the encoder
3.  Choose to send audio or not
4.  Put your generated URL in the box at the bottom
5.  Click the start button
    

## Starting Playback
Depending on the chosen application, follow the directions below.

### VLC 4.0+
1.  Open VLC
2.  Press CTRL + N to open the network stream dialog box
3.  Type in your generated URL
4.  Click Play
    

### OBS Studio
1.  Open OBS
2.  Add a new media source
3.  Name the source and click Ok
4.  Un-check “Local File” box
5.  Paste in the input box your generated URL
6.  Choose to enable/disable any of the other options and click Ok
7.  Your stream should be present allowing for you to resize and position it as you need.