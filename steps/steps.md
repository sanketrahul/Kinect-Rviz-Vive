This project has few requirements:
1. HTC Vive
2. ROS Kinetic or above
3. Ubuntu 16.04
4. openni_camera library installed for Kinect camera
5. Rviz_vive_plugin (https://github.com/AndreGilerson/rviz_vive_plugin) installation and its dependencies like Openvr and steamvr.

Caution: Few of the steps of installation listed at rviz_vive_plugin repository does'nt work or is old. So, you are requested to follow the below steps.

Step:
1. First and biggest thing is to install rviz_vive_plugin. The following steps are inpired from https://github.com/AndreGilerson/rviz_vive_plugin and fruitful discussions on https://github.com/AndreGilerson/rviz_vive_plugin/issues/2 . 
  
  1.1. Approriate drivers for your graphisc card, SDL2 and GLEW. Furthermore git is recommended, for downloading and updating other packages.

    sudo apt install git libsdl2-dev libglew-dev

  1.2. Getting the Robot Operating System (ROS)

To install ROS please follow the instructions provided here ROS kinetic. This plugin was developed using the desktop-full install. After installing ROS, install catkin:

    sudo apt install catkin
    
  1.3. Getting OpenVR

OpenVR can be cloned from the official OpenVR Github repository:

    git clone https://github.com/ValveSoftware/openvr This plugin was developed using OpenVR SDK v1.0.5. Later versions might brake the plugin.
    cd openvr
    git checkout v1.0.5

  1.4.Getting SteamVR

Install the steam client (sudo apt-get install steam) from the Ubuntu repositories. If you have filtered Internet access, you need to run Steam with the -tcp option. Start Steam from a console steam -tcp, login and download SteamVR inside of the Steam Client.

Then, go to the Library -> VR, right click on the SteamVR entry, navigate to Properties -> Local Files -> Browse Local Files and note the path. The path is probably /home/upns/.steam/steam/steamapps/common/SteamVR but may differ.

The plugin was developed using the SteamVR build from January 3rd, 2017. Later versions might brake the plugin. To get older versions of SteamVR, install the Steam command line tool: sudo apt-get install steamcmd. Start it with steamcmd and use following command:

    login
    download_depot 250820 250823 1008772584334738762 This downloads the SteamVR build from January 3rd, 2017. 250820 is the ID of SteamVR, 250823 the ID of the linux depot, and 1008772584334738762 the ID of the correct build. The command line tool will output the path, where it has downloaded the files to, e.g. /home/username/.steam/steamcmd/linux32\steamapps\content\app_250820\depot_250823. Note that Steam was developed for Windows and did not learn that Unix uses slashes for directories. Copy the files into the SteamVR directory you noted earlier, e.g. cp -P steamcmd/linux32/steamapps/content/app_250820/depot_250823/* steam/steamapps/common/SteamVR/ to Merge and replace existing files. [This part of copying different from parent rviz_vive_plugin repository.]
    
  1.5. Build Instructions

The Plugin is built using catkin. You have to create a catkin workspace first. Open up a terminal and:

    mkdir -p catkin_ws/src
    cd catkin_ws/src
    catkin_init_worksspace

Clone this repository into catkin_ws/src. Open the CMakeList.txt file inside of the rviz_vive_plugin folder and change the Path to the OpenVR repository from part 2.3 in line 10. You can then build the plugin by navigating to catkin_ws and executing catkin_make in a terminal.

  1.6. A few steps have to be taken before running the Plugin. First of all, Steam supplies some runtime libraries which are used by SteamVR. Some of those are older versions of commenly used libraries, some of which break RVIZ. Luckily SteamVR does not require those specific libraries, and we can therefore create a folder containing only the required libraries, and add that folder to the LD_LIBRARY_PATH. The Steam runtime can be found in you Steam installation folder. The exact path is based on your operating system (for me it was ~/.steam/ubuntu12_32/steam-runtime/amd64/lib/x86_64-linux-gnu). Create a new folder somewhere, note the path, and copy following files from the Steam runtime folder into the folder you just created:

    libdbus-1.so.3
    libdbus-1.so.3.5.8
    libgpg-error.so.0.8.0
    libncurses.so.5
    libncurses.so.5.9
    libselinux.so.1
    libtinfo.so.5
    libtinfo.so.5.9
    libudev.so.0
    libudev.so.0.13.0
    libwrap.so.0
    libwrap.so.0.7.6
    libz.so.1
    libz.so.1.2.3.4

Now in the rviz_vive_plugin folder again, openup the runtime_setup.sh , and change the path to OpenVR, the SteamVR installation path (PATH_TO_STEAM/steamapps/common/SteamVR), and the Path to the steamruntime folder you just created.

To finally use the plugin, hookup your Vive, startup your ros environment, etc. Open up a new terminal and navigate to your catkin workspace. Then:

    source devel/setup.zsh or source devel/setup.bash depending on your shell
    source src/rviz_vive_plugin/runtime_setup.sh
    If you dont have a roscore started, open up a new terminal and start one with roscore
    chmod a+rw /dev/hidraw *, to give the necessary permissions to read headtracking data from the Vive

Start RVIZ rosrun rviz rviz, then press Add in the bottom left corner of the window, and add the ViveDisplay. Now the view in RVIZ should be rendered to your HTC Vive.

To make the permission change permament, you will have to create a udev rule. Create a new file named vive.rules in /etc/udev/rules.d/:

    sudo gedit /etc/udev/rules.d/vive.rules Paste following rules inside of it and save: KERNEL=="hidraw*", ATTRS{idVendor}=="0bb4", MODE="0666" KERNEL=="hidraw*", ATTRS{idVendor}=="28de", MODE="0666" KERNEL=="hidraw*", ATTRS{idVendor}=="0424", MODE="0666"

2. After installing everything we need to launch kinect on the terminal. So, open a fresh terminal and source the terminal to ROS environments. Then run command: roslaunch openni_launch openni.launch

3. On rviz, set the Fixed Frame (top of the Display panel under Global Options) to /camera_link.

In the Displays panel, click Add and choose the PointCloud2 display. Set its topic to /camera/depth/points. Turning the background color to dark black can help with viewing.

[This step is inspired from http://wiki.ros.org/openni_launch/Tutorials/QuickStart]

4. Now, we can get our required visualization of the robot workspace i,e. Microsoft Kinect output depth images in HTC Vive Headset. Below is the screenshot of our kinect to HTC Vive visualization through rviz. 

![kinect-vive-rviz](https://github.com/sanketrahul/Kinect-Rviz-Vive/blob/master/Image/kinect-vive-rviz.png)
