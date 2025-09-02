---
slug: /8-8-online-contest
title: 'TuyaOpen SDK 快速入门'
authors: [tuya]
image: /img/home/tuyaopen-logo-social-preview.png
tags: [TuyaOpen, Contest]
---
## ## 环境搭建及 SDK 下载

首先，您需要搭建开发环境并下载 TuyaOpen SDK，详细教程请前往 [TuyaOpen 文档中心](https://tuyaopen.ai/zh/docs/about-tuyaopen) 查看。

## 创建产品

前往登录 [涂鸦开发者平台 > **产品开发**](https://platform.tuya.com/pmg/list) 页面，单击 **创建产品**，根据您的产品形态选择品类，并参考 [创建产品](https://developer.tuya.com/cn/docs/iot/create-product?id=K914jp1ijtsfe) 完成产品的创建。

![Create product](/img/blog-images/create-product.png)

进入产品开发流程后，请重点关注下文介绍的添加产品功能、AI 能力和新增固件相关配置。

### 添加产品功能

在 **01 功能定义** > **产品功能** 下，单击 **添加功能** 来为产品添加标准/自定义功能，或开启高级功能。

了解产品功能，请参考 [产品功能](https://developer.tuya.com/cn/docs/iot/define-product-features?id=K97vug7wgxpoq)。

![Add product funtion](/img/blog-images/add-product-function.png)

### 添加 AI 能力

在 **01 功能定义** > **产品 AI 功能** 下，单击 **新增智能体** 来为产品添加 AI 能力。

进入智能体开发流程后，参考 [产品 AI 功能开发](https://developer.tuya.com/cn/docs/iot/AI-feature?id=Keapy1et1fc63) 完成智能体的开发。其中，重点关注以下配置：

![Add AI agent.png](/img/blog-images/add-ai-agent.png)

#### 添加工具集：
1. 在 **01 模型能力配置** > **技能配置** 下选择 **工具集**，单击右侧添加（**+**）按钮进入 **添加工具** 页面。
2. 在 **设备控制** > **设备自控 - 进控制与 Agent 关联的设备** 下，选择添加 **控制智能体绑定的设备**。

![Add tool](/img/blog-images/add-tool-1.png)

![Add tool](/img/blog-images/add-tool-2.png)

#### 开发提示词

在 **02** > **提示词开发** 下，参考 [Prompt 入门教程](https://www.tuyaos.com/viewtopic.php?t=3724) 完成提示词的开发。

![Prompt](/img/blog-images/prompt.png)

### 新增自定义固件

为了后续 OTA 升级和模组批量下单，需要新增自定义固件。

进入产品开发流程后，在 **03 硬件开发** 下，选择 **云端接入开发方式** 为 **TuyaOS AI**，选择 **云端接入硬件** 为 T5 模组，然后单击 **新建自定义固件** 并完成相关配置。

![Add firmware](/img/blog-images/add-firmware.png)


## AI 控制指令配置

:::info
如您在产品开发过程中添加的产品功能均为标准功能（Data Point，DP；标准功能即 DP ID 小于 100 的功能点），则默认已配置自控指令，可以跳过本步骤；如您增加了自定义功能点，则需要完成本步骤修改指令方案。
:::

前往 [**AI 产品指令配置**](https://platform.tuya.com/exp/voice/ai) 页面，在 **自控指令** 下单击 **修改指令方案**，并参考 [设备自控指令](https://developer.tuya.com/cn/docs/iot/Self-control?id=Kep3yhifdrvah) 完成相关配置。 

![AI command](/img/blog-images/edit-command.png)

## 实现产品功能

DP 是 App 控制设备的数据通道，也是智能体控制设备的载体。例如，在产品中添加了开关和温度功能，需要在 TuyaOpen SDK 中解析 DP 数据以实现具体的功能，如下图所示：

![DP data](/img/blog-images/dp-data.png)

## 授权开发板

### 方式一：代码授权

修改头文件。详细流程，请参考 [设备授权](https://tuyaopen.ai/zh/docs/quick-start/equipment-authorization)。

例如，打开 `your_chat_bot` 项目，找到 `tuya_config.h` 文件，路径为：`apps/tuya.ai/your_chat_bot/include/tuya_config.h`，并修改以下三个参数：
- `TUYA_PRODUCT_ID`：产品创建时生成的 Product ID（PID）。
- `TUYA_OPENSDK_UUID`：UUID 可免费获取，请联系涂鸦工作人员领取。
- `TUYA_OPENSDK_AUTHKEY`：Authkey 可免费获取，请联系涂鸦工作人员领取。

![PID](/img/blog-images/pid.png)

示例如下：

```
#ifndef TUYA_PRODUCT_ID
#define TUYA_PRODUCT_ID "p320pepzvmm1ghse"//PID
#endif

#define TUYA_OPENSDK_UUID    "uuidxxxxxxxxxxxxxxxx"             // Please change to the correct UUID.
#define TUYA_OPENSDK_AUTHKEY "keyxxxxxxxxxxxxxxxxxxxxxxxxxxxxx" // Please change to the correct authkey.

```

### 方式二：工具授权

在 **Tuya Uart Tool** 中，选择烧录串口和 **BaudRate**（波特率），单击 **Start** 打开串口，然后单击 **Authorize** 进行授权。

![Tuya Uart Tool](/img/blog-images/tuya-uart-tool.png)

## 编译和烧录

在终端输入以下：

```
tos.py build && tos.py flash 
```

![Result.png](/img/blog-images/result.png)

如果使用 T5AI-Board 开发板，板本身配置有两路串口，一路用于烧录，另一路用于日志输出。如果烧录失败，可以切换端口号并重试。

## 常见问题

### 烧录总是在 Write 时失败，如何解决？

请参考 [安装对应驱动](https://tuyaopen.ai/zh/docs/tos-tools/tools-tyutool#%E7%83%A7%E5%BD%95%E8%BF%87%E7%A8%8B%E4%B8%AD%E6%80%BB%E6%98%AF%E5%9C%A8write%E6%97%B6%E5%A4%B1%E8%B4%A5) 尝试解决。