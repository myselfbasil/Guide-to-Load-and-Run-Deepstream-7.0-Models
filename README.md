<div align="center">
  <p>
    <a align="center" href="" target="_blank">
      <img
        width="850"
        src="https://github.com/myselfbasil/Guide-to-Load-and-Run-PeopleNet-Deepstream-7.0-Model/blob/8eb8a3e5227cae29d4e9dbc28f0d75aa7206bac4/assets/header_img.png"
      >
    </a>
  </p>
</div>

# Guide to Load and Run Peoplenet Deepstream 7.0 Models

**Reference Docs:**

1. https://docs.nvidia.com/metropolis/deepstream/6.0/dev-guide/text/DS_docker_containers.html
2. https://docs.nvidia.com/metropolis/deepstream/6.0/dev-guide/text/DS_ref_app_deepstream.html
3. https://docs.nvidia.com/metropolis/deepstream/6.0/dev-guide/text/DS_sample_custom_gstream.html
4. https://learn.nvidia.com/en-us/training/self-paced-courses

Have a quick reading through the Documentation.

1. **Pull the docker container:**

```bash
$ docker pull [nvcr.io/nvidia/deepstream:7.0-gc-triton-devel](http://nvcr.io/nvidia/deepstream:7.0-gc-triton-devel)
```

1. **Connect the webcam and follow the below commands :)**

```bash
#To run the docker as root:
docker run -it --rm --net=host --gpus all -e DISPLAY=$DISPLAY \
--device /dev/video0 --device /dev/snd \
-v /tmp/.X11-unix/:/tmp/.X11-unix \
-v /etc/nvidia:/etc/nvidia \
-v /opt/nvidia/deepstream/deepstream-7.0/samples/configs:/config \
-v /opt/nvidia/deepstream/deepstream-7.0/samples/models:/models \
-w /opt/nvidia/deepstream/deepstream-7.0/samples/configs/deepstream-app \
[nvcr.io/nvidia/deepstream:7.0-gc-triton-devel](http://nvcr.io/nvidia/deepstream:7.0-gc-triton-devel)
```

1. **Open another separate terminal and type these commands,**

```bash
#To give access to the camera:
$ xhost +
$ gst-launch-1.0 v4l2src device=/dev/video0 ! videoconvert ! autovideosink
```

1. Inside the docker terminal, type this command to edit the deepstream model config file (source1_usb_dec_infer_resnet_int8.txt): 

```bash
$ nano source1_usb_dec_infer_resnet_int8.txt
```

1. Changes made to the config file:

```bash
[source0]
enable=1
#Type - 1=CameraV4L2 2=URI 3=MultiURI
type=1
uri=/dev/video0
camera-width=640
camera-height=480
camera-fps-n=30
camera-fps-d=1
camera-v4l2-dev-node=0
```

1. Command to run the deepstream model inside the docker:

```bash
$ deepstream-app -c source1_usb_dec_infer_resnet_int8.txt
```

**There you go! Now you can see the output.**
