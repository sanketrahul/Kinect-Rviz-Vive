# Kinect-Rviz-Vive
Kinect Point Cloud Visualization to HTC Vive using rviz in Ubuntu
This is the first time that we have achieved visualizing Kinect Depth output in HTC Vive. Previously, people have visualized Point cloud in Windows using Unity Engine. Few examples can be found here:

(i)ROS Reality at Brown: https://www.youtube.com/watch?v=HlRJZYNNndI

(ii) MIT CSAIL work on Teleoperating robots in Virtual Reality:  http://news.mit.edu/2017/mit-csail-new-system-teleoperating-robots-virtual-reality-1009

(iii) Deep imitation learning for complex manipulation tasks from Virtual Reality Teleoperation at UC Berkley: https://arxiv.org/pdf/1710.04615.pdf

The key problem with these visualization is that this process can lose some packets over the internet. There can also be some delay in transfer. So, we thought of coming up with something in ROS using rviz visualization. 

Key Idea of carrying out kinect-rviz-vive visualization:
Currently, through openni libraries, we can visualize point cloud data to the desktop. 
We can leverage this technique to publish point cloud data through RVIZ on the vive node. 
There has been already rviz_vive_plugin (https://github.com/AndreGilerson/rviz_vive_plugin) which can publish any image to HTC Vive headset.
 We have already openni libraries publishing point cloud from Kinect to rviz. We used rviz as bridge to visualize Kinect Depth point cloud to HTC Vive headset through rviz_vive_plugin. Thus, with our smart engineering to rviz_vive_plugin, rviz and openni_launch, we can achieve following visualization:
![kinect-vive-rviz](https://github.com/sanketrahul/Kinect-Rviz-Vive/blob/master/Image/kinect-vive-rviz.png)

The visualiztion on Windows using Unity Engine can be shown below:
![kinect-vive-unity](https://github.com/sanketrahul/Kinect-Rviz-Vive/blob/master/Image/kinect-vive-unity.png)

It is evident that we have achieved very good and clear viualization with rviz than to visualization on Windows machine via Unity engine. Also, the key problem of packet loss over the internet and delay can be solved easily as rviz is just an node on the ROS. Thus, through this project we have achieved a robust and clear visualization of Kinect Depth point cloud (Robot environment) to HTC Vive Headset. This will help a lot to the current researchers working on HTC Vive as input sensors in Human-robot interaction applications.

All the steps to carry out this project is described under steps/steps.md 

Credits: This work has been carried out for Google Summer of Code 2018 under mentorship of Prof. Kei Okada (@k-okada)
