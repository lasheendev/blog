---
title: "Live stream music with ffmpeg and bash"
date: 2023-02-22
description: "How to make your own LOFI radio station with ffmpeg and raspberry pi on youtube"
draft: false
---

In this post, I will show you how to live-stream music videos with an animation.

## What you need
You need an always-on computer with bash installed. I will use a raspberry pi for this example. You can also use a virtual machine for this. Learn how to get a free one from [here](https://blog.lasheen.dev/oracle-cloud-free-vps-4-cpus-24-gb-of-memory/).


## Install ffmpeg
You also need ffmpeg installed. You can install it with `sudo apt install ffmpeg`

## Get the music
Make sure you have the music you want to stream in a folder. I will use the folder `./audio` for this example. Also, make sure that the music is in mp3 format.

## The script
```bash
# This command will remove all spaces and quotes from the file names of the audio files(This can be skipped)
for f in *; do mv -i "$f" "${f//[\"[:space:]]}"; done


# This creates a text file with all the mp3 files' names in the folder audio (This can be skipped if you have the file already)
printf "file '%s'\n" ./audio/*.mp3 > mylist.txt
sed -i '1s/^/ffconcat version 1.0 \n/' mylist.txt


# Stream info
youtubeUrl="rtmp://a.rtmp.youtube.com/live2/xxxx-xxxx-xxxx-xxxx-xxxx"
key="xxxx-xxxx-xxxx-xxxx-xxxx" # Add your key here

framerate=15 # The framerate of the animation
VBR="2500k"       # Bitrate of the video
quality="ultrafast"  # The quality of the video

image="back.gif" # The animation file, can be any gif or video file

# The command that streams the video
ffmpeg -y -ignore_loop 0 -r $framerate -i $image  \
-safe 0 -stream_loop -1 -i mylist.txt -map 1:a:0 -c:v libx264 -preset $quality -b:v $VBR  \
-bufsize 2500k -g $(($framerate * 2)) -crf 17 -c:a aac -b:a 320k -ar 44100  \
-drop_pkts_on_overflow 1 -attempt_recovery 1 -recovery_wait_time 1 -recover_any_error 1 \
-f flv "$youtubeUrl/$key"
```

I tried this script on the [oracle cloud free vm](https://blog.lasheen.dev/oracle-cloud-free-vps-4-cpus-24-gb-of-memory/) and it kept streaming for 3 days without any problems.