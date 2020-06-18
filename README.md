1280 x 720
# capture full screen screen
ffmpeg -video_size 1920x1080 -framerate 25 -f gdigrab -i desktop  output.avi
ffmpeg -video_size 1280x720 -framerate 10 -f gdigrab -i desktop  output.avi
# --> Macos
ffmpeg -f avfoundation -i "1" -pix_fmt yuv420p -r 25 -t 5 out.mov

# capture with region
ffmpeg -f gdigrab -framerate ntsc -offset_x 10 -offset_y 20 -video_size 640x480 -show_region 1 -i desktop output.avi

# get all devices
ffmpeg -list_devices true -f dshow -i dummy
# capture with audio with input device Microphone (Conexant SmartAudio HD)
ffmpeg -f dshow -i audio="Microphone (Conexant SmartAudio HD)" "out.mp4"

# enabled recording device on window before
Stereo Mix (Conexant SmartAudio HD)
# capture video and audio
ffmpeg -f gdigrab -framerate ntsc -video_size 1920x1080 -i desktop  -f dshow -i audio="Microphone (Conexant SmartAudio HD)" -vcodec libx264 -pix_fmt yuv420p -preset ultrafast output.mp4

ffmpeg -f dshow -i audio="Stereo Mix (Conexant SmartAudio HD)" -f dshow -i audio="Microphone (Conexant SmartAudio HD)" -f gdigrab -framerate 10 -video_size 1920x1080 -draw_mouse 1 -i desktop -filter_complex "[0:a][1:a]amerge=inputs=2[a]" -map 2 -map "[a]" -vcodec libx264 -pix_fmt yuv420p -preset ultrafast output.avi

ffmpeg -f dshow -i audio="Stereo Mix (Conexant SmartAudio HD)" -f dshow -i audio="Microphone (Conexant SmartAudio HD)" -f gdigrab -framerate 10 -video_size 1280x720 -draw_mouse 1 -i desktop -filter_complex "[0:a][1:a]amerge=inputs=2[a]" -map 2 -map "[a]" -vcodec libx264 -pix_fmt yuv420p -preset ultrafast -f segment -segment_time 1800 output_%03d.avi

ffmpeg -f gdigrab -framerate ntsc -video_size 1920x1080 -i desktop  -f dshow -i audio="Stereo Mix (Conexant SmartAudio HD)" -vcodec libx264 -pix_fmt yuv420p -preset ultrafast output.mp4

# adjust volumne
ffmpeg  -f dshow -i audio="Stereo Mix (Conexant SmartAudio HD)" -f dshow -i audio="Microphone (Conexant SmartAudio HD)" -f gdigrab -framerate 10 -video_size 1920x1080 -draw_mouse 1 -i desktop -filter_complex "[0:a][1:a]amerge=inputs=2[a]" -filter:a "volume=10dB" -map 2 -map "[a]" -vcodec libx264 -pix_fmt yuv420p -preset ultrafast output.avi

# record only audio
ffmpeg -f dshow -i audio="Stereo Mix (Conexant SmartAudio HD)" output.mp3
ffmpeg -f dshow -i audio="Microphone (Conexant SmartAudio HD)" output.mp3

# split and join video
ffmpeg -i output.avi -acodec copy -f segment -segment_time 10 -segment_list list.txt -vcodec copy -reset_timestamps 1 -map 0 fff%d.avi
ffmpeg -i "concat:fff0.avi|fff1.avi|fff2.avi" -c copy output2.avi
