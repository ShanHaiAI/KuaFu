# Gorilla:大模型连接海量API
[原文链接](https://arxiv.org/abs/2305.15334)  
## 背景
* LLM局限性：
  * 只能依赖存储在固定参数中的信息
  * 只能基于静态图和受限的上下文计算
* LLM需要重新训练来更新知识和推理能力

## APIBench搭建
* API文档收集：
  * 来源：TorchHub、TensorHub和Huggingface(包括多模态数据、CV、NLP、语音、表格数据和强化学习几个主要领域),抓取了1645条API调用方法
  * 数据形式(json)：
    * {domain, framework, functionality, api_name, api_call, api_arguments, environment_requirements, example_code, performance, and
description.}
* 指令生成：
  * 基于self-instruct，通过GPT4生成了调用API的真实用例 根据每条API调用方法，生成了10个不同的用户提问
  * 强调限制使用API名称或者暗示（增加真实性，帮助Gorilla更好的学习）
  * 基于AST子树匹配技术评估查找正确性。例如torch.hub.load的形式，用于检索数据集

## Gorilla
一个LLaMA-7B-based 模型，并且带有API文档数据集的文档检索功能
  * Gorilla在API功能性的准确率上明显比GPT4减少了幻觉的产生
* 带有限制的API调用
  * API调用要求LLM能理解其功能并根据不同的约束参数进行分类
  * 通过提示词调用LLM生成API调用接口，更需要模型能够理解用户的功能性描述和请求中的约束
  * 复杂环境下更需要模型能够根据具体情况进行判断
* 检索感知训练
  * 有助于模型适应API文档更新变化、提高语境学习表现效果并最终降低幻觉
  * 但是检索增强LLM有时也会影响效果
  * 检索感知训练帮助Gorilla更好的适应API文档的变化
* Gorilla的推理能力
