# 论文 Paper
## 0. 背景 Introduction
* 针对agent加强和替代人类工作，目前的解决方案：过于简化了复杂问题，舍弃许多有价值的合作交流。
* SOP(标准化操作文档， Standardized Operating Procedures)可以更好的指导团队工作，提高效率和输出质量。

关于MetaGPT：
> a meta-programming framework for multi-agent collaboration based on LLMs
* 所有agent生成结构化输出，显著提高代码生成的结果；且这种上下游严格的交互方式，显著减少了LLM的幻觉现象。
* 元编程meta-programming：“programing to program”通过编程实现编程。
* 相比于CodeBERT和CodeLlama等方式，MetaGPT通过agents的细化分工，和完整开发流程规划，实现了不同的解决方案。

### 0.1 相关工作
#### 自动化编程 Automatic Programming
* 商业解决方案：微软Copliot
* 基于LLM的agent架构：ReAct + Reflexion + CoT
* ToolFormer：提供API调用外部工具
* 现有角色扮演框架&现有多代理协作软件开发： 存在劣势，没有整合为带有标准输出结构的工作流
#### 基于LLM的多代理框架 LLM-Based Multi-agent Frameworks
* multi-agent通过多个代理间交流讨论，提高了LLM解决问题的能力。
* Stable-Alignment：通过与沙盒中的LLM交互，得出价值判断上的共识，进而生成了指令数据集。
* Generative Agents：斯坦福小镇，25个agents自主相互交互
* 通过多agent之间进行头脑风暴来解决复杂问题
* 通过将大模型作为工具制作者，小模型作为工具使用者，来减少成本。

## 1. METAGPT: A META-PROGRAMMING FRAMEWORK
### 1.1 SOP中的agent
#### 角色专业化
* 定义了5个角色：
  * 产品经理(Product Manager)，架构师(Architect)，项目经理(Project Manager)，工程师(Engineer)，以及测试工程师(QA Engineer)
* 每个角色都有自己的资料(By Prompt)和能力(Tools use)
* 所有agent都遵循ReAct形式的行为
* 每个agent都会监控环境(MetaGPT中的消息池)，用于跟进流程和触发动作。
#### 代理间的工作流
* 通过代理角色和技能配置，基于SOP形成了MetaGPT的工作流程。
* 流程概述：
  * 产品经理：根据用户需求生成PRD(产品需求文档)
  * 架构师：根据需求生成系统设计，如文件列表、数据结构和接口定义等。
  * 项目经理：根据设计分发对应任务
  * 工程师：根据具体需求，开发代码
  * 测试工程师：根据代码，生成测试用例，保障代码质量。
> *Q1：为什么分工协作的效果优于单人？*
> *Q2：agent的角色认同是否有意义？例如取名*
> *Q3：SOP是否为最优解？* Team work！

### 1.2 交流协议
#### 结构化通信接口(Structured Communication Interfaces)
* 针对每个角色和请求，独立提供了必要的输出格式
#### 发布订阅机制(**Publish-Subscribe Mechanism**)
* 提出了消息池机制(message pool)，agent可以发布自己的消息，和获取其他agent发布的消息，也可以在池中检索所需内容，极大提高交流效率。
* 为了避免消息过载，在执行任务期间，agent只接受与任务强相关的消息
* 订阅机制：agent通过角色资料来提取相关信息，而不是仅通过对话。

### 1.3 带有可执行反馈的迭代编程(ITERATIVE PROGRAMMING WITH EXECUTABLE FEEDBACK)
* code-review和self-reflexion机制可以提供代码审阅，但是在确保代码可执行性和执行正确率上仍存在不足。
* 为避免幻觉带来的忽视特定代码错误，提出了可执行的反馈机制来提高代码质量。
* 具体来说，工程师可以根据初始产品需求写代码，再基于代码生成测试用例进行DEBUG，循环往复直到到达最大迭代次数或较高质量代码。
  * 这样有助于工程师agent根据历史指令记录和调试记忆优化代码。

## 2. 实验
* 实验证明MetaGPT的使用效果优于其他方案。
* 通过消融实验证明了角色分配和可执行的反馈机制都有提升了最终生成代码的质量。

## 3. 个人总结
* **基于现有流程设计的Prompt-Engineering-Based Agentic workflow**
  * 一力降十会：给用户提前写好提示词。
  * 不同视角：把整个workflow抽象成一个公司，用户体感实际为black box
* **教会LLM干活**
  * LLM有生产能力，有逻辑能力，但是不会干活（结合实际来生成更合理的逻辑来完成工作）
  * 因此要结合现有的工作逻辑，“教会”LLM干活
* 缺点or问题
  * 提示词重度依赖，如果后续基础LLM持续优化，可能被取代。
  * 针对制定场景设计提示词，泛用性差
* 值得借鉴的地方：
  * SOP流程：把现实中的开发流程搬到LLM应用里。role-play+react+self-refine
* 包括RAG在内的部分直接参考、引用了llama-index