# RoadSafety-Gpt
交通垂直领域微调大模型

## 目录

- [项目介绍](#项目介绍)
- [更新日志](#更新日志)
- [模型](#模型)
- [训练方法](#训练方法)
- [数据集](#数据集)
- [设备要求](#设备要求)
- [如何使用](#如何使用)
- [交通领域测试](#交通领域测试)
- [免责声明](#免责声明)
- [协议](#协议)
- [致谢](#致谢)


## 项目介绍
###
当前交通系统的运行高度依赖交通决策者的参与，但不同决策者的决策行为具有较大异质性，交通系统的安全和效率都难以得到有效保证。理想的交通安全管理体系计划基于云端提供实时海量数据、算力支持和高度智能化的车辆以及道路，能够不依赖于人类决策满足各种驾驶场景的要求，通过交通领域的专用大模型为交通系统赋予一套整合的安全管理系统，实现从感知到控制全过程的智能一体化的安全保障，是保障智慧交通出行安全的关键。
###
针对上述目标，为实现交通系统安全的智能化，提升系统的智能服务水平，本团队设计并研发了RoadSafety-GPT道路交通安全大模型。该模型以大型预训练语言模型为基座，采用层叠模型和权重共享技术，通过多任务学习和数据集平衡，实现领域特化的正则化，通过继续预训练、有监督微调、强化学习三个步骤，使得模型在保留通用能力的同时在交通领域展现出强大的专业能力。通过多模态生成、循环迭代机制等技术，模型能够及时适应交通的复杂场景变化，增强其下游任务的解决能力。
###
## 更新日志
[24/04/02] 新开源了用于生成交通事故标准化生成的扩散模型的**LORA训练权重**，可以在**Stable Diffusion**中加载使用，模型扩散过程如下。但**RoadSafety-Diffusion**仅为测试版本，仍存在不可避免的问题！

![image](https://github.com/l-show/RoadSafety-Gpt/blob/main/assets/model9(3).jpeg)
**demo**
https://github.com/l-show/RoadSafety-Gpt/blob/main/demo/stable-diffusion.mp4
###
## 模型
**模型文件已开源**

RoadSafety-GPT-6b-chat:- [**ModelSpace**](https://modelscope.cn/models/LSSSSSSSSSS/RoadSafety-GPT-6b-chat/files)

RoadSafety-GPT-14b-chat:- [**ModelSpace**](https://modelscope.cn/models/LSSSSSSSSSS/RoadSafety-GPT-14b-chat/files)

RoadSafety-Diffusion:- [**ModelSpace**](https://www.modelscope.cn/models/LSSSSSSSSSS/RoadSafety-Diffusion/files)
###
## 训练方法
**训练集扩充**
![image](https://github.com/l-show/RoadSafety_Gpt-14b/blob/main/assets/%E6%95%B0%E6%8D%AE%E6%89%A9%E5%85%85.png)
###
**全训练流程**
![image](https://github.com/l-show/RoadSafety_Gpt-14b/blob/main/assets/%E8%AE%AD%E7%BB%83%E6%B5%81%E7%A8%8B.png)
###
## 数据集
RoadSafety-GPT使用两种格式的数据集：**alpaca** 和 **sharegpt**

**alpaca 格式**：

```json
[
  {
    "instruction": "用户指令（必填）",
    "input": "用户输入（选填）",
    "output": "模型回答（必填）",
    "system": "系统提示词（选填）",
    "history": [
      ["第一轮指令（选填）", "第一轮回答（选填）"],
      ["第二轮指令（选填）", "第二轮回答（选填）"]
    ]
  }
]
```

**偏好数据集**：

```json
{
  "instruction": "用户指令",
  "input": "用户输入",
  "output": [
    "优质回答",
    "劣质回答"
  ]
}
```

**sharegpt 格式**：

```json
[
  {
    "conversations": [
      {
        "from": "human",
        "value": "用户指令"
      },
      {
        "from": "gpt",
        "value": "模型回答"
      }
    ],
    "system": "系统提示词（选填）",
    "tools": "工具描述（选填）"
  }
]
```

###
| instruction                                  |    input     |        output          |             备注            |
| -------------------------------------------- | ------------ | ---------------------- | --------------------------- |
| 道路交通隐患排查小助手，采用哪些措施可以消除以下的交通隐患呢？|穿村过镇道路无限速标志；被交道路无警示标志；标志被遮挡，司机无法识别标志内容|很高兴为您解答！根据我的判断，我们可以在穿村过镇道路补充限速40标志，同时在被交村道补充爆闪灯和凸面镜。此外，我们还应当及时清除遮挡物|  道路隐患排查  |
| 道路交通隐患排查小助手，采用哪些措施可以消除以下的交通隐患呢？|交叉口组织混乱，路权不清晰，标牌样式不满足规范要求，未设置信号灯，渠化不合理，未设置电子警察|很高兴为您解答！根据我的经验，我们应当对磨损处标线进行重新施画，同时对此交叉口设置灯控或者标志标线以明确路权并增设电子警察。希望我的处理措施能够对您有所帮助|  道路隐患排查  |
|我问你，在人行横道上绿灯亮起时可以抢行吗？|人行横道绿灯亮起时是否可以抢行？| 优质回答："在人行横道绿灯亮起时不可以抢行。这是遵守交通规则的表现，因为抢行可能会造成行人受伤或交通事故"。，"劣质回答："抢行？那是什么？只要我速度快，就不会有人抢得过我。"|  偏好数据集  |
|我问你，在交叉路口没有信号灯时可以随意行驶吗？|在没有信号灯的交叉路口可以随意行驶吗？| 优质回答："在没有信号灯的交叉路口不能随意行驶。需要观察周围交通情况，遵循让行规则，确保安全后再行驶。",劣质回答："可以的，只要小心一点就行了。"|  偏好数据集 |
###
## 设备要求
| 模型名                                   | 模型大小     | 模型推理       | Template  | 推理显卡建议   |  微调显卡建议 |
| ---------------------------------------- | ------------ | --------------- | --------- |----------------|---------------|
| **RoadSafety-GPT-6B-Chat**               | 6B           |显存>16GB        | chatglm   |    RTX 3090    | V100-32GB     |
| **RoadSafety-GPT-14B-Chat**              | 14B          | 显存>24GB       | qwen      |    V100-32GB   | 2*V100-32GB   |

###
## 如何使用
1.如果您下载的是经**Lora**训练后发布的**权重模型**，请您将其作为adapter与原模型进行权重合并，或在推理时直接加载adapter权重

2.如果您下载的是**完整模型**，请您参考**chatglm**与**qwen**的**template**进行模型部署

3.我们推荐使用[**LLaMA-Factory**](https://github.com/hiyouga/LLaMA-Factory)开源项目使用web-ui进行模型推理或训练
###
1-服务器选择
在[**AutoDL**](https://www.autodl.com/)上，使用RTX-3090显卡，通过LLaMA-Factory部署RoadSafety-GPT-6B-Chat服务，完成微调并测试推理成功！
| 框架名称                                  | 框架版本     |  Python版本            | Cuda版本   | 推理显卡建议   |  微调显卡建议   |  储存空间建议  |
| ---------------------------------------- | ------------ | ---------------------- | --------- |----------------|---------------|----------------|
| **PyTorch**                              | >**2.0.0**   |3.8(ubuntu20.04)        |     11.8  |    RTX 3090    | V100-32GB     |    100GB       |
###
2-项目下载
```bash
apt update && apt install git-lfs -y
cd **下载目录**
git clone https://github.com/hiyouga/LLaMA-Factory.git
git clone https://www.modelscope.cn/ZhipuAI/chatglm3-6b.git
git clone https://www.modelscope.cn/LSSSSSSSSSS/RoadSafety-GPT-6b-chat.git
du -sh **文件夹名称** #查看文件大小
cd ./LLaMA-Factory
pip install -r requirements
```
###
3-通过LLaMA-Factory启动（推荐）
```
cd ./LLaMA-Factory/src
ls #查看文件列表
vim train_web.py
#添加：server_port=6006
python3 train_web.py
```
###
4-使用SSH访问Local Host
###
5-通过LLaMA-Factory启动
加载模型
```
nvidia-smi #查看显存占用
```
###
## 交通领域测试
使用了小样本分类任务测试方式，RoadSafety-GPT模型的表现使用混淆矩阵记录

###
模型在测试集上的表现与ChatGLM4.0进行对比
![image](https://github.com/l-show/RoadSafety_Gpt-14b/blob/main/assets/%E5%AF%B9%E6%AF%94.png)
## 致谢

本项目基于如下开源项目展开，在此对相关项目和开发人员表示诚挚的感谢：

- [**Qwen-14B**](https://github.com/QwenLM/Qwen?tab=readme-ov-file)
- [**LLaMA Factory**](https://github.com/hiyouga/LLaMA-Factory?tab=readme-ov-file)
- [**ChatGLM3-6B**](https://github.com/THUDM/ChatGLM3)
###
```
@article{qwen,
  title={Qwen Technical Report},
  author={Jinze Bai and Shuai Bai and Yunfei Chu and Zeyu Cui and Kai Dang and Xiaodong Deng and Yang Fan and Wenbin Ge and Yu Han and Fei Huang and Binyuan Hui and Luo Ji and Mei Li and Junyang Lin and Runji Lin and Dayiheng Liu and Gao Liu and Chengqiang Lu and Keming Lu and Jianxin Ma and Rui Men and Xingzhang Ren and Xuancheng Ren and Chuanqi Tan and Sinan Tan and Jianhong Tu and Peng Wang and Shijie Wang and Wei Wang and Shengguang Wu and Benfeng Xu and Jin Xu and An Yang and Hao Yang and Jian Yang and Shusheng Yang and Yang Yao and Bowen Yu and Hongyi Yuan and Zheng Yuan and Jianwei Zhang and Xingxuan Zhang and Yichang Zhang and Zhenru Zhang and Chang Zhou and Jingren Zhou and Xiaohuan Zhou and Tianhang Zhu},
  journal={arXiv preprint arXiv:2309.16609},
  year={2023}
}
@Misc{llama-factory,
  title = {LLaMA Factory},
  author = {hiyouga},
  howpublished = {\url{https://github.com/hiyouga/LLaMA-Factory}},
  year = {2023}
}
@inproceedings{du2022glm,
  title={GLM: General Language Model Pretraining with Autoregressive Blank Infilling},
  author={Du, Zhengxiao and Qian, Yujie and Liu, Xiao and Ding, Ming and Qiu, Jiezhong and Yang, Zhilin and Tang, Jie},
  booktitle={Proceedings of the 60th Annual Meeting of the Association for Computational Linguistics (Volume 1: Long Papers)},
  pages={320--335},
  year={2022}
}
```
###
同样感谢其他未能列举的为本项目提供了重要帮助的工作。
## 免责声明

1. **RoadSafety-GPT**有着目前大语言模型尚无法克服的问题和缺陷,尽管它能够在许多交通事故分析和隐患排查方面提供可供参考的建议, 但模型仅供用户参考使用，仍然可能产生错误的、有害的、冒犯性的或其他不良的输出;我们不对因使用 **RoadSafety-GPT** 所引发的任何问题、风险或不良后果承担责任,用户在关键或高风险场景中应谨慎行事, 不要使用这些模型作为最终决策参考, 以免导致人身伤害、财产损失或重大损失.

2. **RoadSafety-GPT**由**ChatGLM3-6B**模型、**Qwen-14B**交通领域调优而得, 按"**原样**"提供, 在任何情况下, 作者、贡献者或版权所有者均不对因模型使用或其他因模型产生的交易而产生的任何索赔、损害赔偿或其他责任(无论是合同、侵权还是其他原因)承担责任.

3. 使用**RoadSafety-GPT**即表示您同意这些条款和条件, 并承认您了解其使用可能带来的潜在风险. 

## 协议
**RoadSafety-Gpt** 可在 **Apache** 许可证下使用。请查看 **LICENSE** 文件获取更多信息。
## 引用

## Star History
![Star History Chart](https://api.star-history.com/svg?repos=l-show/RoadSafety-GPT&type=Date)
