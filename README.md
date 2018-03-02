
##AudioGrep
AudioGrep use https://github.com/antiboredom/audiogrep

Download audio file from youtube

`youtube-dl URL --write-auto-sub -f mp3`

if case download didn't specify format convert to MP3

`ffmpeg -i input_video.ext -acodec libmp3lame audio.mp3`


Transcibe audio to txt

`audiogrep --input path/to/*.mp3 --transcribe`

Create supercut of audio clips of phrases containing search words

`audiogrep --input audio.mp3 --search 'pattern'`

Specify output mode

`audiogrep --input audio.mp3 --search 'pattern' --output-mode=word`

or other options, create a regex sentence with crossfade of half a milisecond

`audiogrep --input audio.mp3 --search 'artificial|city|becomes' --regex --output-mode word -o 'artificial_city_becomes.mp3' -c 500`

##VideoGrep

VideoGrep use https://antiboredom.github.io/videogrep/

Convert .mpv to .mp4

https://askubuntu.com/questions/396883/how-to-simply-convert-video-files-i-e-mkv-to-mp4

`ffmpeg -i input.mkv -acodec ac3 -vcodec copy video.mp4`

https://ffmpeg.zeranoe.com/forum/viewtopic.php?t=468

convert .vtt to srt

`ffmpeg -i input.vtt video.srt`

run search on video

`videogrep --input video.mp4 --search 'city'`

use n-gram for occurence frequency

 `videogrep --input video.mp4 --ngrams 3 ` << number of words


## PATTERNs

for a more accurate transcript use audiogrep/pocketsphinx

`videogrep --input video.mp4 -transcribe`

then we can do POS tags with the .mp4.transcription.txt file

`videogrep --input video.mp4 --search 'JJ' --use-transcript  --search-type pos`
`videogrep --input video.mp4 --search '^VBG DT JJ NN' --use-transcript --search-type pos`
`videogrep --input video.mp4 --search 'but' --use-transcript --search-type word`
Incase ffmpeg crash before compiling all the temp tiles together

`ffmpeg -f concat -safe 0 -i <(for f in ./*.mp4; do echo "file '$PWD/$f'"; done) -c copy output.mp4`


## Fillers

Youtube subtiles adds a padding to start & end time to each phrase
so that the endtime of the prev phase is actually greater than then start time of the next phrase to ensure more natural clipping. Due to this overlap we can't grab fillers from this subtitle file

Use AUTOSUB for more accurate timestamp

https://github.com/agermanidis/autosub

`autosub -o subs.srt -F srt video.mp4` remember to rename .srt to the same name as .mp4


SILENCE PY

`python silence.py --input video.mp4 --output silence.mp4`
