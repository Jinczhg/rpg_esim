# ESIM: an Open Event Camera Simulator

[![ESIM: an Open Event Camera Simulator](http://rpg.ifi.uzh.ch/esim/img/youtube_preview.png)](https://youtu.be/ytKOIX_2clo)

This is the code for the 2018 CoRL paper **ESIM: an Open Event Camera Simulator** by [Henri Rebecq](http://henri.rebecq.fr), [Daniel Gehrig](https://danielgehrig18.github.io/) and [Davide Scaramuzza](http://rpg.ifi.uzh.ch/people_scaramuzza.html):
```bibtex
@Article{Rebecq18corl,
  author        = {Henri Rebecq and Daniel Gehrig and Davide Scaramuzza},
  title         = {{ESIM}: an Open Event Camera Simulator},
  journal       = {Conf. on Robotics Learning (CoRL)},
  year          = 2018,
  month         = oct
}
```
You can find a pdf of the paper [here](http://rpg.ifi.uzh.ch/docs/CORL18_Rebecq.pdf). If you use any of this code, please cite this publication.

## Python Bindings
Python bindings for the event camera simulator can be found [here](https://github.com/uzh-rpg/rpg_vid2e). 
We now also support GPU support for fully parallel event generation!


## Features

- Accurate event simulation, guaranteed by the tight integration between the rendering engine and the event simulator
- Inertial Measurement Unit (IMU) simulation
- Support for multi-camera systems
- Ground truth camera poses, IMU biases, angular/linear velocities, depth maps, and optic flow maps
- Support for camera distortion (only planar and panoramic renderers)
- Different C+/C- contrast thresholds
- Basic noise simulation for event cameras (based on additive Gaussian noise on the contrast threshold)
- Motion blur simulation
- Publish to ROS and/or save data to rosbag

## Install

Installation
Tested with Ubuntu 20.04 and ROS Noetic.

### Prequisite
pcl_ros, glm, glfw, and (optional) hector-trajectory-server

Create a new catkin workspace:

`mkdir -p ~/sim_ws/src && cd ~/sim_ws`

Install vcstools if you do not have it already:

`sudo apt-get install python-vcstool`

Clone this repository and run vcstools:

`cd src/`

`git clone https://github.com/Jinczhg/rpg_esim.git`

`vcs-import < rpg_esim/dependencies.yaml`

Disable the packages that are not needed:

    cd ze_oss
    touch imp_3rdparty_cuda_toolkit/CATKIN_IGNORE \
          imp_app_pangolin_example/CATKIN_IGNORE \
          imp_benchmark_aligned_allocator/CATKIN_IGNORE \
          imp_bridge_pangolin/CATKIN_IGNORE \
          imp_cu_core/CATKIN_IGNORE \
          imp_cu_correspondence/CATKIN_IGNORE \
          imp_cu_imgproc/CATKIN_IGNORE \
          imp_ros_rof_denoising/CATKIN_IGNORE \
          imp_tools_cmd/CATKIN_IGNORE \
          ze_data_provider/CATKIN_IGNORE \
          ze_geometry/CATKIN_IGNORE \
          ze_imu/CATKIN_IGNORE \
          ze_trajectory_analysis/CATKIN_IGNORE

Build the assimp_catkin node:

`catkin build assimp_catkin`  or `catkin_make --pkg assimp_catkin `
      
Build the event_camera_simulator node:

`catkin build esim_ros` or `catkin_make --pkg esim_ros`

Build the event renderer node (for event data visualization):

`catkin build dvs_renderer` or `catkin_make --pkg dvs_renderer`

Make an alias for your workspace so you can source it easily next time.

`echo "source ~/sim_ws/devel/setup.bash" >> ~/setupeventsim.sh`

`chmod +x ~/setupeventsim.sh`

In your .bashrc file, add the following line:

`alias ssim='source ~/setupeventsim.sh'`

From now on, typing ssim in a terminal will initialize the simulator workspace (you need to run bash first if you want to try this right after editing the .bashrc file.

## Run

Specific instructions to run the simulator depending on the chosen rendering engine can be found in [our wiki](https://github.com/uzh-rpg/rpg_esim/wiki).

## Acknowledgements

We thank Raffael Theiler and Dario Brescianini for their contributions to ESIM.
This research was supported by by Swiss National Center of Competence Research Robotics (NCCR), Qualcomm (through the Qualcomm Innovation Fellowship Award 2018), the SNSF-ERC Starting Grant and DARPA FLA.

A significant part of ESIM uses components (spline trajectories, inertial measurement unit simulation, various utility functions) from the [ze_oss](https://github.com/zurich-eye/ze_oss) project.
ESIM depends on [UnrealCV](https://github.com/unrealcv/unrealcv) for the photorealistic rendering engine.
We also reused some [code samples](https://github.com/JoeyDeVries/LearnOpenGL.git) from the excellent [Lean OpenGL](https://learnopengl.com/) tutorial in our OpenGL rendering engine.
Finally, ESIM depends on the [Open Asset Import Library (assimp)](https://github.com/assimp/assimp) to load 3D models and Blender scenes within the OpenGL rendering engine.
