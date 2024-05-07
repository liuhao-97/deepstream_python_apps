This folder is a modified version of the original deepstream-test1 app. The original README is at the end of this file.

The receiver(Jetson AGX Orin) receives h264 video from sender(PC) by udp, execute pgie, and show video with result on screen.

The Jetson run

``
python3 udp-in-pgie-screen.py
``

Then RUN on sender:

``
gst-launch-1.0 -v filesrc location=/opt/nvidia/deepstream/deepstream-6.3/samples/streams/sample_720p.h264 ! h264parse ! rtph264pay ! udpsink host=10.68.187.189 port=5400
``

You should see video with objective detector result on the screen.


![pipeline](https://github.com/liuhao-97/deepstream_python_apps/blob/nvaie-3.0/apps/udp-in-pgie-screen/pipeline.png)



# The original README

Prequisites:
- DeepStreamSDK 6.2
- Python 3.8
- Gst-python

To run the test app:
  $ python3 deepstream_test_1.py <h264_elementary_stream>

This document shall describe the sample deepstream-test1 application.

It is meant for simple demonstration of how to use the various DeepStream SDK
elements in the pipeline and extract meaningful insights from a video stream.

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
file "gstnvdsmeta.h"

