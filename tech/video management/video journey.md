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

Here is an example command I would use: `'ffmpeg -vaapi_device /dev/dri/renderD128 -i "{show_name_and_episode}" -x265-params "pass=1" -c:v hevc_amf -rc cqp -qp_i 20 -qp_p 20 -an -f null /dev/null && ffmpeg -vaapi_device /dev/dri/renderD128 -i "{show_name_and_episode}" -map_metadata 0:g -x265-params "pass=2" -c:v hevc_amf -rc cqp -qp_p 20 -qp_i 20 -c:a aac "{transcoded_show_name}"'`

This is the end result of my work and I am going to break down step by step. 

First of all, you can do [Two pass encoding](https://trac.ffmpeg.org/wiki/Encode/H.264). *If you scroll down on the link it will show a Two Pass example.* Two pass encoding allows ffmpeg to run its encoders over the file multiple times in order to get a better encoded file. In this case a 'better' file is usually smaller, but without loss in quality. 'Quality' is hard for me to define for my experience in video and audio codecs. So I will leave it up to people with more experience then me.

# First Pass:

`ffmpeg -vaapi_device /dev/dri/renderD128 -i "{show_name_and_episode}" -x265-params "pass=1" -c:v hevc_amf -rc cqp -qp_i 20 -qp_p 20 -an -f null /dev/null`

Hopefully this is much more managable :). The `-vaapi_device` is just my graphics card. `-i` refers to the source video container. `-x265-params` is just a HEVC thing in ffmpeg to signify the first pass. `-c:v` signifies the video codec used for the output file, which in this case is AMD HEVC. 

I am going to skip over the `-rc`, `-qp_i`, and `-qp_p` arguements becuase that goes really deep into the rabbit hole of video quality and how encoder settings can cause artifacting or other unpleasent things to show up in videos. Here is a link to the page I used to get those [arguements](https://github.com/GPUOpen-LibrariesAndSDKs/AMF/wiki/Recommended-FFmpeg-Encoder-Settings#rate-control) if you want to read more about Constant QP. But it is an older technology and is being converted into a much easier metric like [Constant Rate Factor](https://slhck.info/video/2017/02/24/crf-guide.html) or `crf`.

If you want an easier way to manage quality, I would reccomend something like profiles in gpu codecs. It is a much easier place to start

The last arguements are just to signify it is a first pass and ffmpeg does not need to make an output container yet, since their is going to be a second pass.

# Second Pass:

`ffmpeg -vaapi_device /dev/dri/renderD128 -i "{show_name_and_episode}" -map_metadata 0:g -x265-params "pass=2" -c:v hevc_amf -rc cqp -qp_p 20 -qp_i 20 -c:a aac "{transcoded_show_name}"`

Most of the arguements are the same as the first pass, so I am only going to cover the new parameters. `-map_metadata` is a subset of the `-map` parameter. Basically ffmpeg allows you to control where each bitstream is within a video container. The defaults are 0 for video, 1 for audio, and 2 for subtitle. The next two sections elaborate more on this parameter, so I am going to move on with the other parameters. `-c:a` refers to the audio codec. You can just leave it out if you do not want to force re-encode audio codecs. Like I do for subtitles here `-c:s copy`. And lastly it is just the output filename for the video container. Yaaay. Now you can at least reference this if you get confused on what parameters mean.

# Example of remapping streams on twitch

I mentioned this above when talking about the -map parameter. Basically [Twitch](https://twitch.tv) allows its creators to have two audio streams on its website. An example mapping is 0 video and 1 & 2 are audio. But Twitch will only save audio stream 1 in the recorded VOD. So it looks like this 0 and 1. A lot of creators have realized that you can basically stream any copyrighted audio as long as it is on the second audio stream. So a ton of VODs have creators listening to music during 'Just Chatting' segments and sadly the VOD watchers do not get to enjoy the music :(. I am not sure if their is a similar thing with [Youtube](https://youtube.com) or [Kick](https://kick.com) streaming.

# Not removing globabl metadata in video containers

This happened a lot before I started using the `-map_metadata` parameter. A lot of my old encoded video containers lost dates, chapter segements, encoder info etc. because I was not using this parameter.

# Hard Subbing vs. Soft Subbing

Basically 'Hard Subbing' is encoding the subtitle text into the actual video frame where as 'Soft Subbing' is just adding an extra subtitle stream to the video container. I believe that Soft Subbing is superior and should be used for all source files. I like to do everything I can to perserve video quality and Hard Subbing destroys video frames. I do sometimes have to use Hard Subbing when I am trying to share a video containers, but I only ever do that on the last encode before I share the file.

# Problems I found with HEVC

The biggest problem with HEVC is support. HEVC is owned by [MPEG](https://www.mpeg.org/standards/MPEG-H/2/) and each company that wants to support HEVC has to pay a license fee to get access to the codecs encoder and decoder. Because of this fee no browser supports playback of HEVC through its decoder. Which means Youtube's, Netflix's, and Social medias like Discord, twitter etc cannot play this video codec. The video container has to be downloaded or a special player like [VLC](https://www.videolan.org/vlc/), [MPV](https://mpv.io/) or another one needs to be used. 

On Windows, if you do not want to download mpv or VLC you have to install [this](https://apps.microsoft.com/detail/9nmzlz57r3t7?hl=en-US&gl=US) to get the windows media player to support HEVC.

# Consequences of HEVC and the birth of AV1

As a result of HEVC being locked behind a paywall, this stifled video on the internet. The big players like Google basically bought out the [VP9](https://en.wikipedia.org/wiki/VP9) codec and had their own special codec for YouTube. But as for other companies like Netflix and Vimeo they had to use H.264 which is really [old](https://en.wikipedia.org/wiki/Advanced_Video_Coding) or pay for HEVC. So eventually a lot of the big companies got together and made AV1 and the [AOM](https://aomedia.org/). Because the point of all of these websites is not to sell you a video codec, but each big company wanted an amazing experience, without each company having to pay MPEG's fee for HEVC. For context H.264 is from 2003 and H.265 is from 2013. So every web company streaming video was eager for a lot of improvements to how videos are stored and shared.

# Switching to AV1

I also wanted to start using AV1 becuase Jellyfin has a web player too, but I could only watch the HEVC videos on device's with HEVC support. I ended up finding this gpu from intel called the [Intel Arc Pro A40](https://www.intel.com/content/www/us/en/products/sku/230317/intel-arc-pro-a40-graphics/specifications.html), which was around $200. Where AMD's and Nvidia's cards were way more expensive. I did run into a huge issue that I had a lot of HEVC encoded video and H.264. So I needed a way to convert them to AV1. 

I installed the intel gpu into my main desktop and tried pointing ffmpeg towards the intel card instead of the AMD card. And I ran into a bunch of issues, since not a lot of people run 2 gpu's on a desktop and its even rarer to use AMD and Intel.

# Switching to using Handbrake 

To temporarily solve the issue above I started using [Handbrke](https://handbrake.fr). This project is super nice and helped me visual what the ffmpeg commands above were doing. The program supports 2 GPU setups very easily and has a lot of improvements over just using ffmpeg commands. I have manually converted over a bunch of my HEVC videos to AV1 and I can play most of my media library through web players now. Yaay!!!

# Making a video transcoding server

From the mess of converting HEVC to AV1, I wanted to find a way to automate the process. I basically settled on making a brand new pc for my intel card and converting my two GPU desktop into two 1 GPU desktops. I also made sure it had a full itx motherboard, so I may start running other home services on it. It is starting as a humble transcoding server, but it has the potential to be so much more :).

# Using both Opus and AAC audio codecs

I have done a little experimenting with the [Opus audio codec](https://opus-codec.org/) and it is really cool. It is supported just like [AAC](https://en.wikipedia.org/wiki/Advanced_Audio_Coding) but the opus codec is better for streaming because of the support of way more bitrates than AAC. For my home lab purposes, I think AAC and OPUS are probably equal in terms of usefulness, but I understand why YouTube and Discord prefer OPUS over AAC.

# Using Lossless Cut to more easily make clips

[Lossless Cut](https://mifi.no/losslesscut)

# Sharing my videos through MP4 video containers rather than Matroska



# Things I want to still explore

- Subtitle codecs [Subtitle codecs](https://www.matroska.org/technical/subtitles.html)
- Audio Codecs [FLAC](https://en.wikipedia.org/wiki/FLAC)
- How constant Quality versus targeting a specific bitrate affects video quality
- How to make better encodes for sharing with friends
- Storing off old project files in a small file size while still perserving quality

