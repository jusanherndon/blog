I have a humble beginnings when I started learning about video containers, video codecs, audio codecs, and subtitles. I wanted to host a media server and have the media I want to watch wherever I go.

# Starter hardware

I started out with my [AMD Radeon RX 6750XT](https://www.amd.com/en/products/graphics/desktops/radeon/6000-series/amd-radeon-rx-6750-xt.html), becuase it was the gpu I got a deal with when I built my first ever desktop.

It does not have a lot of fancy software features becuase its an AMD card. 

# Starter Codecs and Containers

I started with [H.264](https://trac.ffmpeg.org/wiki/Encode/H.264), because that is the most prevalent video out there. 

I also had the [Subrip](https://en.wikipedia.org/wiki/SubRip) format to save the subtitle I had gotten from movies that I had digitized.

I saved a lot of the audio in [AAC](https://trac.ffmpeg.org/wiki/Encode/AAC), because that was the default.

Lastly, I googled which video conatiner to use and a lot of people pointed me to [Matroska](https://en.wikipedia.org/wiki/Matroska) video containers, since they are open source and support basically every codec under the sun. 

# Motivation to try newer things

I really did not like the size of the video files I was working with. About 20 mins of video was about 1.5 Gbs and I was curious if their was a way to shrink down the size. 

I did some more research and found out that audio and subtitles do not really take up a lot of space in video containers. 

*Sidenote from Future Justin: You could maybe make an arguement for [FLAC](https://en.wikipedia.org/wiki/FLAC) or [WAV](https://en.wikipedia.org/wiki/WAV) audio files, but I was nowhere near that deep into audio codecs at that time.*

So I continued on with my video codec research. I ended up finding [HEVC](https://trac.ffmpeg.org/wiki/Encode/H.265) or H.265 and [AV1](https://trac.ffmpeg.org/wiki/Encode/AV1). But sadly my GPU did not support AV1 encode, so I was forced to choose HEVC.

# Dark ages of transcoding

Well, I wanted to shrink those video containers, so I learned how to [transcode](https://en.wikipedia.org/wiki/Transcoding) via [ffmpeg](https://ffmpeg.org/) terminal commands. 

Here is an example command I would use: 'ffmpeg -vaapi_device /dev/dri/renderD128 -i "{show_name_and_episode}" -x265-params "pass=1" -c:v hevc_amf -rc cqp -qp_i 20 -qp_p 20 -an -f null /dev/null && ffmpeg -vaapi_device /dev/dri/renderD128 -i "{show_name_and_episode}" -map_metadata 0:g -x265-params "pass=2" -c:v hevc_amf -rc cqp -qp_p 20 -qp_i 20 -c:a aac "{transcoded_show_name}"'

This is the end result of my work and I am going to break down step by step. 

First of all, you can do [Two pass encoding](https://trac.ffmpeg.org/wiki/Encode/H.264). *If you scroll down on the link it will show a Two Pass example.* Two pass encoding allows ffmpeg to run its encoders over the file multiple times in order to get a better encoded file. In this case a 'better' file is usually smaller, but without loss in quality. 'Quality' is hard for me to define for my experience in video and audio codecs. So I will leave it up to people with more experience then me.

# First Pass:

ffmpeg -vaapi_device /dev/dri/renderD128 -i "{show_name_and_episode}" -x265-params "pass=1" -c:v hevc_amf -rc cqp -qp_i 20 -qp_p 20 -an -f null /dev/null 

Hopefully this is much more managable :). The -vaapi_device is just my graphics card. -i refers to the source video container. -x265-params is just a HEVC thing in ffmpeg to signify the first pass. -c:v signifies the video codec used for the output file, which in this case is AMD HEVC. 

I am going to skip over the -rc, -qp_i, and -qp_p arguements becuase that goes really deep into the rabbit hole of video quality and how encoder settings can cause artifacting or other unpleasent things to show up in videos. Here is a link to the page I used to get those [arguements](https://github.com/GPUOpen-LibrariesAndSDKs/AMF/wiki/Recommended-FFmpeg-Encoder-Settings) if you want to read more about Constant QP. But it is an older technology and is being converted into a much easier metric like [Constant Rate Factor](https://slhck.info/video/2017/02/24/crf-guide.html) or crf.

If you want an easier way to manage quality, I would reccomend something like profiles in gpu codecs. It is a much easier place to start

The last arguements are just to signify it is a first pass and ffmpeg does not need to make an output container yet, since their is going to be a second pass.

# Second Pass:

ffmpeg -vaapi_device /dev/dri/renderD128 -i "{show_name_and_episode}" -map_metadata 0:g -x265-params "pass=2" -c:v hevc_amf -rc cqp -qp_p 20 -qp_i 20 -c:a aac "{transcoded_show_name}"


