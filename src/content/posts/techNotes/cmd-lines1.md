---
title: 常用计算机指令——第1弹
published: 2025-03-28
tags: [Tech Notes]
category: Tech Notes
draft: false
lang: zh
---

# NNI自动调参

## 什么是NNI？

**NNI (Neural Network Intelligence)** 是MSRA开源的一个自动机器学习（AutoML）工具包。它旨在帮助用户自动进行特征工程、神经网络架构搜索、超参数调优以及模型压缩。NNI为AI开发者提供了一个高效的实验管理平台，通过其丰富的调优算法和可视化界面，大大简化了机器学习模型优化的复杂过程。

## NNI的主要功能

- 🔍 **超参数调优**：支持多种先进的搜索算法，如贝叶斯优化、进化算法和强化学习等
- 🏗️ **神经网络架构搜索**：自动寻找最优网络结构
- 🧩 **特征工程**：自动化特征选择和生成
- 📊 **实验管理**：提供Web界面，方便追踪和比较不同实验结果
- 📉 **模型压缩**：支持剪枝、量化等压缩技术，减小模型体积并提高推理速度

## NNI常用命令

### 📥 安装NNI

```bash
# 使用pip安装
pip install nni

# 从源码安装
git clone https://github.com/microsoft/nni.git
cd nni
python setup.py install

```

### 🚀 启动实验
```bash
# 启动一个新实验
nnictl create --config config.yml

# 使用指定端口启动实验
nnictl create --config config.yml --port 8088
```

### 📊 管理实验
```bash
# 查看所有实验
nnictl experiment list

# 查看特定实验详情
nnictl experiment show [experiment_id]

# 停止实验
nnictl stop [experiment_id]

# 恢复实验
nnictl resume [experiment_id]

# 导出实验结果
nnictl experiment export [experiment_id] --filename [file_path]
```

### 📋 查看实验日志
```bash
# 查看实验Webui日志
nnictl log stdout [experiment_id]

# 查看实验错误日志
nnictl log stderr [experiment_id]

# 查看特定trial的日志
nnictl log trial [trial_id]
```

### 🔬 实验配置
*注意，在启动实验前需要先配置好一个config.yaml文档*
```yaml
# config.yaml基本结构
authorName: default
experimentName: example_experiment
trialConcurrency: 1
maxExecDuration: 1h
maxTrialNum: 10
searchSpace:
  lr:
    _type: choice
    _value: [0.0001, 0.001, 0.01, 0.1]
  optimizer:
    _type: choice
    _value: ['Adam', 'SGD']
trial:
  command: python model.py
  codeDir: .
  gpuNum: 0
tuner:
  builtinTunerName: TPE
  classArgs:
    optimize_mode: maximize
```

### 🐍 python代码设置
```python
import nni

# 获取NNI分配的超参数
params = nni.get_next_parameter()
lr = params['lr']
optimizer = params['optimizer']

# 向NNI报告指标
acc = train_model(lr, optimizer)
nni.report_final_result(acc)
```