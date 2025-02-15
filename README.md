# mcl_unknown_obstacle_experiment [![Build](https://github.com/CIT-Autonomous-Robot-Lab/mcl_unknown_obstacle_experiment/actions/workflows/build.yaml/badge.svg)](https://github.com/CIT-Autonomous-Robot-Lab/mcl_unknown_obstacle_experiment/actions/workflows/build.yaml) [![Docker](https://github.com/CIT-Autonomous-Robot-Lab/mcl_unknown_obstacle_experiment/actions/workflows/docker.yaml/badge.svg)](https://github.com/CIT-Autonomous-Robot-Lab/mcl_unknown_obstacle_experiment/actions/workflows/docker.yaml)

観測範囲による未知障害物の対策を実装した[emcl2](https://github.com/CIT-Autonomous-Robot-Lab/emcl2/tree/feat/observation-range)を使用して、シミュレータ環境上で
動作させるためのパッケージ

# Install

* GPUなしの場合

```
docker run --rm -it \
           -u $(id -u):$(id -g) \
           --privileged \
           --net=host \
           --ipc=host \
           --env="DISPLAY=$DISPLAY" \
           --mount type=bind,source=/home/$USER/.ssh,target=/home/$USER/.ssh \
           --mount type=bind,source=/home/$USER/.gitconfig,target=/home/$USER/.gitconfig \
           --mount type=bind,source=/usr/share/zoneinfo/Asia/Tokyo,target=/etc/localtime \
           --name mcl-unknown-obstacle-experiment \
           ghcr.io/cit-autonomous-robot-lab/mcl_unknown_obstacle_experiment:melodic
```

`gazebo`コマンドの起動後に以下のようなエラーが出る場合は**GPUありの場合**の`docker run`をするとうまく行くかもしれません
```
libGL error: No matching fbConfigs or visuals found
libGL error: failed to load driver: swrast
Error: couldn't get an RGB, Double-buffered visual
```

* GPUありの場合

```
docker run --rm -it \
           -e NVIDIA_VISIBLE_DEVICES=all \
           -e NVIDIA_DRIVER_CAPABILITIES=all \
           --gpus all \
           -u $(id -u):$(id -g) \
           --privileged \
           --net=host \
           --ipc=host \
           --env="DISPLAY=$DISPLAY" \
           --mount type=bind,source=/home/$USER/.ssh,target=/home/$USER/.ssh \
           --mount type=bind,source=/home/$USER/.gitconfig,target=/home/$USER/.gitconfig \
           --mount type=bind,source=/usr/share/zoneinfo/Asia/Tokyo,target=/etc/localtime \
           --name mcl-unknown-obstacle-experiment \
           ghcr.io/cit-autonomous-robot-lab/mcl_unknown_obstacle_experiment:melodic
```

# Launch file Description

* mcl_experiment.launch

| option                   | description                           | type   | default   |
| ------------------------ | ------------------------------------- | ------ | --------- |
| world_name               | シミュレータ環境の名前                | string | tsudanuma |
| unknown_obstacle         | 未知障害物を配置するか                | bool   | true      |
| handle_unknown_obstacles | 未知障害物対策有りのemcl2を実行するか | bool   | true      |
| observation_range        | 観測範囲[deg]                         | int    | 30        |

# Run

* 千葉工業大学 津田沼校舎シミュレータ（未知障害物有りの場合）
```
roslaunch mcl_unknown_obstacle_experiment mcl_experiment.launch \
  world_name:=tsudanuma \
  unknown_obstacle:=true \
  handle_unknown_obstacles:=true \
  observation_range:=30
```

* 千葉工業大学 津田沼校舎シミュレータ（未知障害物無しの場合）
```
roslaunch mcl_unknown_obstacle_experiment mcl_experiment.launch \
  world_name:=tsudanuma \
  unknown_obstacle:=false \
  handle_unknown_obstacles:=false
```

* つくば市役所前シミュレータ（未知障害物有りの場合）
```
roslaunch mcl_unknown_obstacle_experiment mcl_experiment.launch \
  world_name:=tsukuba \
  unknown_obstacle:=true \
  handle_unknown_obstacles:=true \
  observation_range:=20
```

* つくば市役所前シミュレータ（未知障害物無しの場合）
```
roslaunch mcl_unknown_obstacle_experiment mcl_experiment.launch \
  world_name:=tsukuba \
  unknown_obstacle:=false \
  handle_unknown_obstacles:=false
```

# Desktop PC specs used
* 12th Gen Intel® Core™ i9-12900K × 24
* GeForce RTX 2060

# License
Apache License, Version 2.0に基づいています。