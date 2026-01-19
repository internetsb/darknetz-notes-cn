![DarkneTZ](darknetz_logo.png)

This is an application that runs several layers of a Deep Neural Network (DNN) model in TrustZone.
<em><u>这是一个在TrustZone中运行深度神经网络（DNN）若干层的应用程序。</u></em>

This application is based on [Darknet DNN framework](https://pjreddie.com/darknet/) and needs to be run with [OP-TEE](https://www.op-tee.org/), an open source framework for Arm TrustZone.
<em><u>本应用程序基于[Darknet DNN框架](https://pjreddie.com/darknet/)，需要配合Arm TrustZone的开源框架[OP-TEE](https://www.op-tee.org/)一起运行。</u></em>

---------------------------
Please consider citing this corresponding paper at [MobiSys 2020](https://www.sigmobile.org/mobisys/2020/) if this project is helpful to you:
<em><u>如果此项目对您有帮助，请考虑引用以下[MobiSys 2020](https://www.sigmobile.org/mobisys/2020/)会议论文：</u></em>

**[DarkneTZ: Towards Model Privacy at the Edge using Trusted Execution Environments](https://arxiv.org/abs/2004.05703)** Fan Mo, Ali Shahin Shamsabadi, Kleomenis Katevas, Soteris Demetriou, Ilias Leontiadis, Andrea Cavallaro, Hamed Haddadi
<em><u>**[DarkneTZ：利用可信执行环境实现边缘模型隐私](https://arxiv.org/abs/2004.05703)** Fan Mo, Ali Shahin Shamsabadi, Kleomenis Katevas, Soteris Demetriou, Ilias Leontiadis, Andrea Cavallaro, Hamed Haddadi</u></em>


# Prerequisites
<em><u>前置条件</u></em>
You can run this application with real TrustZone or a simulated one by using QEMU.
<em><u>您可以使用真实的TrustZone或通过QEMU模拟的方式来运行此应用程序。</u></em>

**Required System**: Ubuntu-based distributions
<em><u>**系统要求**：基于Ubuntu的发行版</u></em>

**For simulation**, no additional hardware is needed.
<em><u>**对于模拟环境**，不需要额外的硬件。</u></em>

**For real TrustZone, an additional board is required**. Raspberry Pi 3, HiKey Board, ARM Juno board, etc. Check this [List](https://optee.readthedocs.io/en/latest/building/devices/index.html#device-specific) for more info.
<em><u>**对于真实TrustZone，需要额外的开发板**。树莓派3、HiKey开发板、ARM Juno开发板等。更多信息请查看此[List](https://optee.readthedocs.io/en/latest/building/devices/index.html#device-specific)。</u></em>

# Setup
<em><u>安装配置</u></em>
## (1) Set up OP-TEE
<em><u>(1) 配置OP-TEE</u></em>
1) Follow **step1** ~ **step5** in "**Get and build the solution**" to build the OP-TEE solution.
<em><u>1) 按照 "**获取并构建解决方案**" 中的 **步骤1** ~ **步骤5** 来构建OP-TEE解决方案。</u></em>
https://optee.readthedocs.io/en/latest/building/gits/build.html#get-and-build-the-solution

2) **For real boards**: If you are using boards, keep follow **step6** ~ **step7** in the above link to flash the devices. This step is device-specific.
<em><u>2) **对于真实开发板**：如果您使用的是开发板，请继续按照上述链接中的**步骤6** ~ **步骤7**来刷写设备。此步骤因设备而异。</u></em>

   **For simulation**: If you have chosen QEMU-v7/v8, run the below command to start QEMU console.
<em><u>   **对于模拟环境**：如果您选择了QEMU-v7/v8，请运行以下命令来启动QEMU控制台。</u></em>
```
make run
(qemu)c
```

3) Follow **step8** ~ **step9** to test whether OP-TEE works or not. Run:
<em><u>3) 按照 **步骤8** ~ **步骤9** 测试OP-TEE是否正常工作。运行：</u></em>
```
tee-supplicant -d
xtest
```

Note: you may face OP-TEE related problem/errors during setup, please also free feel to raise issues in [their pages](https://github.com/OP-TEE/optee_os).
<em><u>注意：在安装过程中您可能会遇到OP-TEE相关的问题/错误，请随时在[他们的页面](https://github.com/OP-TEE/optee_os)上提出问题。</u></em>

## (2) Build DarkneTZ
<em><u>(2) 构建DarkneTZ</u></em>
1) clone codes and datasets
<em><u>1) 克隆代码和数据集</u></em>
```
git clone https://github.com/mofanv/darknetz.git
git clone https://github.com/mofanv/tz_datasets.git
```
Let `$PATH_OPTEE$` be the path of OPTEE, `$PATH_darknetz$` be the path of darknetz, and `$PATH_tz_datasets$` be the path of tz_datasets.
<em><u>令 `$PATH_OPTEE$` 为OPTEE的路径，`$PATH_darknetz$` 为darknetz的路径，且 `$PATH_tz_datasets$` 为tz_datasets的路径。</u></em>

2) copy DarkneTZ to example dir
<em><u>2) 将DarkneTZ复制到示例目录</u></em>
```
mkdir $PATH_OPTEE$/optee_examples/darknetz
cp -a $PATH_darknetz$/. $PATH_OPTEE$/optee_examples/darknetz/
```

3) copy datasets to root dir
<em><u>3) 将数据集复制到根目录</u></em>
```
cp -a $PATH_tz_datasets$/. $PATH_OPTEE$/out-br/target/root/
```

4) rebuild the OP-TEE
<em><u>4) 重新构建OP-TEE</u></em>

**For simulation**, to run `make run` again.
<em><u>**对于模拟环境**，再次运行 `make run`。</u></em>

**For real boards**, to run `make flash` to flash the OP-TEE with `darknetz` to your device.
<em><u>**对于真实开发板**，运行 `make flash` 将带有 `darknetz` 的OP-TEE刷入您的设备。</u></em>

5) after you boot your devices or QEMU, test by the command 
<em><u>5) 在您启动设备或QEMU后，使用以下命令测试</u></em>
```
darknetp
```
Note: It is NOT `darknetz` here for the command.
<em><u>注意：此处命令不是 `darknetz`。</u></em>

You should get the output:
<em><u>您应该得到以下输出：</u></em>
 ```
# usage: ./darknetp <function>
 ```
Awesome! You are ready to run DNN layers in TrustZone.
<em><u>太棒了！您已经准备好在TrustZone中运行DNN层了。</u></em>

# Train Models
<em><u>训练模型</u></em>

1) To train a model from scratch 
<em><u>1) 从零开始训练模型</u></em>
```
darknetp classifier train -pp_start 4 -pp_end 10 cfg/mnist.dataset cfg/mnist_lenet.cfg
```
You can choose the partition point of layers in the TEE by adjusting the argument `-pp_start` and `-pp_end`. Any sequence layers (first, middle, or last layers) can be put inside the TEE.
<em><u>您可以通过调整参数 `-pp_start` 和 `-pp_end` 来选择TTE中的分层点。任何连续的层（第一层、中间层或最后一层）都可以放在TEE内部。</u></em>

Note: you may suffer from insufficient secure memory problems when you run this command. The preconfigured secure memory of darknetz is `TA_STACK_SIZE = 1*1024*1024` and `TA_DATA_SIZE = 10*1024*1024` in `ta/user_ta_header_defines.h` file. However, the maximum secure memory size can differ from different devices (typical size is 16 MiB) and maybe not enough. When this happens, you may want to either, configure the needed secure memory to be smaller, or increase the secure memory of the device (for QEMU go to the [link here](https://github.com/OP-TEE/optee_os/issues/2079)).
<em><u>注意：当您运行此命令时，可能会遇到安全内存不足的问题。darknetz的预配置安全内存为 `ta/user_ta_header_defines.h` 文件中的 `TA_STACK_SIZE = 1*1024*1024` 和 `TA_DATA_SIZE = 10*1024*1024`。然而，最大安全内存大小可能因不同设备而异（典型大小为16MiB），可能不够用。发生这种情况时，您可以将所需的安全内存配置得更小，或增加设备的安全内存（对于QEMU，请访问[这里](https://github.com/OP-TEE/optee_os/issues/2079)）。</u></em>

When everything is ready, you will see output from the Normal World like this:
<em><u>当一切准备就绪时，您将看到来自普通世界的如下输出：</u></em>
```
# Prepare session with the TA
# Begin darknet
# mnist_lenet
# 1
layer     filters    size              input                output
    0 conv      6  5 x 5 / 1    28 x  28 x   3   ->    28 x  28 x   6  0.001 BFLOPs
    1 max          2 x 2 / 2    28 x  28 x   6   ->    14 x  14 x   6
    2 conv      6  5 x 5 / 1    14 x  14 x   6   ->    14 x  14 x   6  0.000 BFLOPs
    3 max          2 x 2 / 2    14 x  14 x   6   ->     7 x   7 x   6
    4 connected_TA                          294  ->   120
```