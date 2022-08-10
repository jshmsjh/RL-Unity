# RL-Unity

## Introduction

This is part of a study project using Reinforcement Learning to achieve automated parking in the simulation environment of [Unity](https://unity.com/). We have also wrote a simple survey of DL in Automated driving [ [Web](http://116.62.132.196/) | [Code](https://github.com/wangyimai/avs-report)] .

##### Why Unity?

简而言之，Unity相较于Python或Matlab，提供的环境可以更好的模拟**阿克曼结构**的小车，也可以更好的模拟物理世界场景下小车的运动（碰撞，转向，加减速等等），更贴近**真实世界环境**。

We choose Unity as the working environment for that it could simulated the **real word dynamics** of a car that follows **Ackermann (阿克曼)** steering geometry. Moreover, It provides APIs to use Python or C# as backend, which is easy to use. 

## Reproduction

### Environment

```
PyTorch==1.12+cu116
tensorboard==2.9.0
python==3.8.12
mlagents==0.29.0
```

### Setup Unity and python

1. **Install Unity Hub**. The download link can be find here [ [EN](https://unity3d.com/get-unity/download)  [CN](https://unity.cn/releases) ].  
2. **Install Unity and open the project**. After installing Unity Hub, you can download this repository, then click `Projects-Open` button to open the project in this repository. After that, you can start the process of downloading Unity packages **(version 2020.3.37f1c1 or later**). It may have warnings about the version when opening the project, but it's OK to ignore it. 
3. **Install Python**. The required python packages are list in the `Environment` section. If you have already installed it, you can skip this step.
4. **Verify installation**. Open the console in the root of the project, input `mlagents-learn` command. There should be an Unity logo in the console after the above steps.

### Inference

1. Step 1, Open the RL-Unity project in Unity Hub. Click `Enviornment-CarAgent` in the `Hierarchy` tab which is on the left side of the window.
2. Step 2, Check the `Inspector` tab on the right side of the window, Unfold `Behavior Parameters` tab, and choose the trained DNN model (model_1/model_2). 
3. Step 3, Click `run` button which appears to be a triangle, then the car model should be trying to park it self.

### Training

The Training process is very similar to the Inference. 

1. Step 1, unselect the trained DNN model described in step 2 of the `Inference` section.

2. Step 2, Open the console, and type in :

    ```
    mlagents-learn training_settings_v1.yaml --run-id="PPO" --quality-level=0
    ```

3. Step 3, Click `run` button which appears to be a triangle, then the car model should be trying to train itself, imitating the demonstration located in `Assets/Demos`.

The training settings can be found at `training_settings_v1.yaml`. Parameter tuning is needed in order to make the training converge. 

Please note that the **initial state matters**, the theoretical best position is shown below. 

<img src="C:/Users/15740/AppData/Roaming/Typora/typora-user-images/image-20220810223818378.png" alt="image-20220810223818378" style="zoom:50%;" />

#### Training in parallel

Unity allows parallel training by simply duplicating the training environment. In this project, we duplicated the training environment into 25 copies, which is set to be invisible by default.

#### Visualization

After training for a while, you may use the Tensorboard tool to visualize the loss curves. An example is given below (the training time is 7h).

`tensorboard --logdir result`

![img](file:///F:\Tencent Files\1574074497\Image\C2C\Image1\@%(`NFT)CI_`B_[A90EU[DW.png)

## Results

![](Misc/demo.gif)

## References

This project utilized part of the code and environment from this fantastic [work](https://github.com/VanIseghemThomas/AI-Parking-Unity). Some of the models (mostly cars) is retrieved from the [Unity Asset Store](https://assetstore.unity.com/). We also learned a lot from [this](https://ww2.mathworks.cn/help/reinforcement-learning/ug/train-ppo-agent-for-automatic-parking-valet.html) demo by MathWorks. The referred resources are listed below

[1] Proximal Policy Optimization Algorithms https://arxiv.org/abs/1707.06347

[2] Dueling Network Architectures for Deep Reinforcement Learning http://proceedings.mlr.press/v48/wangf16.html

[3] Mathworks https://ww2.mathworks.cn/help/reinforcement-learning/ug/train-ppo-agent-for-automatic-parking-valet.html

[4] VanIseghemThomas, https://github.com/VanIseghemThomas/AI-Parking-Unity

[5] AI-AutoPark https://github.com/LDH0094/AI-AutoPark

[6] ML-Agents tool kit https://github.com/Unity-Technologies/ml-agents

