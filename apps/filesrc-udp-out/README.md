This folder is changed from deepstream_test1_rtsp_out.

file-source -> h264-parser -> rtppay -> udpsink

The sender(PC) sends h264 video from filesrc to the receiver(Jetson AGX Orin) by udp.

NOTICE there is no nvinfer because I don't have GPU on sender.

The Jetson run

``
gst-launch-1.0 -v udpsrc port=5400 caps="application/x-rtp, media=(string)video, clock-rate=(int)90000, encoding-name=(string)H264, payload=(int)96" ! rtph264depay ! h264parse ! nvv4l2decoder ! autovideosink
``

Then RUN on sender:

``
python3 filesrc-udp-out.py -i /opt/nvidia/deepstream/deepstream-6.3/samples/streams/sample_720p.h264
``


![pipeline](https://github.com/liuhao-97/deepstream_python_apps/blob/nvaie-3.0/apps/filesrc-udp-out/pipeline.png)




# The original README


Prequisites:
- DeepStreamSDK 6.2
- Python 3.8
- Gst-python
- GstRtspServer

Installing GstRtspServer and instrospection typelib

$ sudo apt update
$ sudo apt-get install libgstrtspserver-1.0-0 gstreamer1.0-rtsp
For gst-rtsp-server (and other GStreamer stuff) to be accessible in
Python through gi.require_version(), it needs to be built with
gobject-introspection enabled (libgstrtspserver-1.0-0 is already).
Yet, we need to install the introspection typelib package:
$ sudo apt-get install libgirepository1.0-dev
$ sudo apt-get install gobject-introspection gir1.2-gst-rtsp-server-1.0


To get test app usage information:
  $ python3 deepstream_test1_rtsp_out.py -h
  
To run the test app with default settings:
  $ python3 deepstream_test1_rtsp_out.py -i <h264_elementary_stream>
  


This document shall describe the sample deepstream-test1-rtsp-out application.

It is meant for simple demonstration of how to use the various DeepStream SDK
elements in the pipeline and extract meaningful insights from a video stream.
The output video data with inference output overlayed is encoded and streamed
using GstRtspServer.

This sample creates instance of "nvinfer" element. Instance of
the "nvinfer" uses TensorRT API to execute inferencing on a model. Using a
correct configuration for a nvinfer element instance is therefore very
important as considerable behaviors of the instance are parameterized
through these configs.

For reference, here are the config files used for this sample :
1. The 4-class detector (referred to as pgie in this sample) uses
    dstest1_pgie_config.txt

In this sample, we first create one instance of "nvinfer", referred as the pgie.
This is our 4 class detector and it detects for "Vehicle , RoadSign, TwoWheeler,
Person".
nvinfer element attach some MetaData to the buffer. By attaching
the probe function at the end of the pipeline, one can extract meaningful
information from this inference. Please refer the "osd_sink_pad_buffer_probe"
function in the sample code. For details on the Metadata format, refer to the
file "gstnvdsmeta.h".

