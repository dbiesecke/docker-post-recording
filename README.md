# docker-post-recording

Watches for .ts files made by Live TV recordings, convert them to a friendly format, extract .srt file, add chapters with comchap or remove them with comcut.
Tested with Emby recordings.

## Example run

```shell
docker run -d \
	--name=post-recording \
	-v /docker/appdata/post-recording:/config:rw \
	-v /home/user/videos:/watch:rw \
	-v /home/user/temp:/temp:rw \
	-v /home/user/backup:/backup:rw \
	-e DELETE_TS=1 \
	-e SUBTITLES=0 \
	-e CONVERSION_FORMAT=mkv \
	-e SOURCE_EXT=ts \
	-e POST_PROCESS=comchap \
	-e SOURCE_STABLE_TIME=10 \
	-e PUID=99 \
	-e PGID=100 \
	-e UMASK=000 \
	--restart always \
	chacawaca/post-recording
```

Where:

- `/config`: This is where the application stores its configuration, log and any files needing persistency. 
- `/watch`: This location contains .ts files that need converting. Other files are not processed.  
- `/temp`: This location contains the temporary files that are created during transcoding, etc.
- `/backup`: Optional, only used if DELETE_TS is set to 2.
- `DELETE_TS`: After converting remove the original .ts recording file. 0 = Yes, 1 = No, 2 = Move to backup directory **USE DELETE_TS=1 UNTIL YOU'RE SURE IT WORKS WITH YOUR VIDEO RECORDINGS.**
- `SUBTITLES`: Extract subtitles to .srt. 0= Yes, 1 = No
- `CONVERSION_FORMAT`: Select output extension, your custom.sh need to be valid for this extension.
- `SOURCE_EXT`: If you want to convert something else than .ts
- `POST_PROCESS`: option are comchap or comcut. default: comchap
- `SOURCE_STABLE_TIME`: When a new recording file is found, check every x seconds to see if the recording has completed. default: 10
- `PUID`: ID of the user the application runs as.
- `PGID`: ID of the group the application runs as.
- `UMASK`: Mask that controls how file permissions are set for newly created files.

## Configuration: 

- /scripts/custom.sh **need to be configured** by you, some example are there to help you configure this for your need.
- /hooks can be configured to execute custom code
- /comskip/comskip.ini can be configured too.

## Unraid Users

**[Help with Intel](https://forums.unraid.net/topic/77943-guide-plex-hardware-acceleration-using-intel-quick-sync/)**  
Intel GPU Use  
Install the "Intel GPU TOP" and "GPU Statistics" plugins (unraid 6.9+)
add --device=/dev/dri to "extra parameters" (switch on advanced view)  

**[Help with Nvidia](https://forums.unraid.net/topic/77813-plugin-linuxserverio-unraid-nvidia/)**  
Nvidia GPU Use  
Using the Unraid Nvidia Plugin to install a version of Unraid with the Nvidia Drivers installed and  
add --runtime=nvidia to "extra parameters" (switch on advanced view) and  
copy your GPU UUID to NVIDIA_VISIBLE_DEVICES.  (-e NVIDIA_VISIBLE_DEVICES=GPU-XXXXXXXXXXXXXX)

## projects used

www.github.com/djaydev/docker-recordings-transcoder  
www.github.com/BrettSheleski/comchap  
www.github.com/erikkaashoek/Comskip  
www.github.com/jlesage/docker-handbrake  
www.github.com/ffmpeg/ffmpeg  
www.github.com/CCExtractor/ccextractor  
www.github.com/linuxserver/docker-baseimage-ubuntu  
www.github.com/jrottenberg/ffmpeg
