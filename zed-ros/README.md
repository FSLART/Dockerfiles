# ZED + ROS

- Ubuntu 22.04
- CUDA 11.8

## Build Instructions

- Run `docker build -t fslart/zed-ros .`.
- Start the container using `docker run -it --gpus all fslart/zed-ros`.
- Build the ZED ROS wrapper by running `source /opt/ros/humble/setup.bash && colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release --parallel-workers 6`.
- Exit the container, check its ID using `docker ps --all` and run `docker commit --change 'CMD /bin/bash -c "source /opt/ros/humble/setup.bash && source /zed-ros/install/setup.bash && ros2 launch zed_wrapper zed_camera.launch.py camera_model:=zed2i"' <my-container-id> fslart/zed-ros`.
- Now you can push to the repository if you wish.