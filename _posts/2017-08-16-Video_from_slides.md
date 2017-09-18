---
layout: post
title: "Creating videos from images and audio using ffmpeg"
author: Michael Ringenberg
date: 2017-08-16
---

I was tasked with composing some mp4 videos given a bunch of images
and audio files. Fortunately, the audio files and images were nicely
paired such that the desired result was to have one image per audio
file and concatenating them together to make a single video. It was
essentially making a audio overlaid slide show with one audio file
and one image per slide. Additionally, each of the images for a given
video were the same size (after a small bit of editing). It is
important to note that the dimensions of the image should be even
numbers of pixels.

The tools I ended up using for this included:
* ffmpeg
* mp4art
* svgexport

The first step was converting the SVG images to png using svgexport as ffmpeg
and mp4art can not deal with SVG images. This was fairly easily done
with one caveat: svgexport can handle processing multiple images per
invocation, but if more than five or six images were done, it would
fail with an out of memory error message. Thus my scripts simply
converted the images one at a time. The loss in efficiency was not
significant enough to worry about it, particularly when I incorporated
this as a task into a gradle build which resulted in automatic
out-of-date checking. 

The next step was to compose the images and audio together on a per
slide basis. Using ffmpeg, this is fairly straight forward given the
right command line arguments. For each slide, the command was

```
ffmpeg -loop 1 -i <image-n.png> -i <audio-n.mp3> -shortest -pix_fmt yuv420p -y <slide-n.mp4>
```

Now, I am not an expert with ffmpeg, so I do not know what all of the
options mean and I should probably come back and edit this after
looking those up.

As far as I was able to find out, the recommended way of concatenating
movie files like this is to use .ts file so the next step is to
convert the resulting .mp4 file into a .ts file. The reason for the
conversion is that the .ts library can not handle a static image
"looped" to match the audio but the mp4 library can.

```
ffmpeg -i <slide-n.mp4> -c copy -bsf:v h264_mp4toannexb -f mpegts -y
<slide-n.ts>
```

Now to concatenate them together:
```
ffmpeg -i concate:slide-1.ts|slide-2.ts|...|slide-n.ts -c:v libx264 -c:a aac -movflags +faststart -fflags +genpts -async 1 -strict -2 <movie.mp4>
```

One thing I discovered was necessary was the ```-c:a acc``` and
```-fflags +genpts``` flags. These were required to keep the audio and
videos in sync. If a straight copy of the audio and video was done, it
would work fine on a number of video players with just a little bit of
lag, but on Chrome's HTML5 default video player, the lag between the
audio and video would become much more pronounced in proportion to the
number of files concatenated together.

Finally, setting the proper preview image was the last problem to
address. ffmpeg seemed to pick one at random but the first frame was
the desired image. Now, it would make sense that there would be some
way to do this using ffmpeg given that it can do just about everything
else; however, there is not and the issue to resolve it has not yet
been addressed. This is where mp4art enters the process:
```
mp4art -z --add <image-1.png> <movie.mp4>
```

Writing the scripts that does this over the forty plus movies is
left as an exercise for the reader.
