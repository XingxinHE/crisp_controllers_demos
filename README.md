# CRISP Controllers Demos

<img src="https://github.com/user-attachments/assets/284983f8-2311-4699-86ab-06fc2ea9d5af" alt="CRISP Controllers Logo" width="160" align="right"/>

<a href="https://github.com/utiasDSL/crisp_controllers_demos/actions/workflows/docker_build.yml"><img src="https://github.com/utiasDSL/crisp_controllers_demos/actions/workflows/docker_build.yml/badge.svg"/></a>
<img alt="Static Badge" src="https://img.shields.io/badge/arxiv-cite-b31b1b?style=flat&link=google.com">
<a href="https://utiasDSL.github.io/crisp_controllers/"><img alt="Static Badge" src="https://img.shields.io/badge/docs-passing-blue?style=flat&link=https%3A%2F%2FutiasDSL.github.io%2Fcrisp_controllers%2F"></a>

This repo provides Docker containers to provide directly test the [crisp_controllers](https://github.com/utiasDSL/crisp_controllers) with real hardware or in simulation with a simple [MuJoCo](https://github.com/google-deepmind/mujoco) `ros2_control` interface provided in this repository.

Check the [docs](https://utiasdsl.github.io/crisp_controllers/misc/demos/) on how to get started with the demos and with CRISP in general.



RMW=cyclone ROS_NETWORK_INTERFACE=lo ROS_DOMAIN_ID=100 docker compose up --build launch_franka
or
RMW=cyclone ROS_NETWORK_INTERFACE=lo ROS_DOMAIN_ID=100 docker compose up launch_franka

## SpaceMouse teleop (with `franka_spacemouse`)

After launching `launch_franka`, switch controllers so Cartesian teleop can command
`target_pose`:

```bash
docker compose exec launch_franka bash -lc "cd /home/ros/ros2_ws && source /opt/ros/humble/setup.bash && source install/setup.bash && export RMW=cyclone && export ROS_NETWORK_INTERFACE=lo && export ROS_DOMAIN_ID=100 && source ./src/crisp_controllers_demos/scripts/setup_middleware.sh && ros2 control switch_controllers --deactivate joint_trajectory_controller --activate cartesian_impedance_controller"
```

Then, from `franka_spacemouse`, run:

```bash
export RMW=cyclone
export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
export ROS_NETWORK_INTERFACE=lo
export ROS_DOMAIN_ID=100
export CYCLONEDDS_URI=file://$PWD/config/cyclone_config.xml

cd ../franka_spacemouse
pixi run build
pixi run bash -lc "source install/setup.bash && ros2 launch spacemouse_publisher spacemouse_publisher.launch.py config_file:=example_crisp_controllers_demo_fr3.yaml"
```