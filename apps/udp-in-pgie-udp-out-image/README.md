This folder is a modified version of the original deepstream-rtsp-in-rtsp-out. The original README is at the end of this file.

1. The sender (PC) sends video from filesrc to Jetson by udp.

2. Jetson receives video by udp, executes pgie, print results, and send back to sender by udp. 

3. The sender connects to screen, receives video with object detection results, and shows on screen.


hamza: docker send video by udp-out 5400

``
gst-launch-1.0 -v filesrc location=/opt/nvidia/deepstream/deepstream-6.3/samples/streams/sample_720p.h264 ! h264parse ! rtph264pay ! udpsink host=10.68.187.189 port=5400
``

Jetson AGX Orin: udp-in 5400 -> pgie -> udp-out 5410
``
python3 udp-in-pgie-udp-out.py
``

hamza (not docker): udp-in 5410 -> screen 

``
gst-launch-1.0 -v udpsrc port=5410 caps="application/x-rtp, media=(string)video, clock-rate=(int)90000, encoding-name=(string)H264, payload=(int)96" ! rtph264depay ! h264parse ! avdec_h264 ! autovideosink
``



![pipeline](https://github.com/liuhao-97/deepstream_python_apps/blob/nvaie-3.0/apps/udp-in-pgie-udp-out/pipeline.png)

