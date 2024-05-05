# Ai Agent
### 目录
- 定义:什么是Agent?
    - 大模型的本质
    - Agent的定义
    - 为什么LLM可以实现Agent
- 应用:怎么实现Agent? 
    - 论文:ReAct
    - 通过Langchain实现Agent
    - 应用产品:MetaGPT
- 意义:Agent的本事是什么?
    - CoT和ReAct的区别
    - Agent的本质还是对LLM的应用
- 延申:如何去优化Agent?
    - 模型微调:更适合Agent的LLM

## 定义:什么是Agent?
### 大模型的本质
- ChatGPT是什么?
    - ChatGPT是一种基于GPT技术的自然语言处理模型,其中GPT是指Generative Pre-trained Transformer 的简称。ChatGPT可以用于生成自然流畅、连贯的对话,并可以根据上下文内容提供的提示来生成完整有意义的回复。
    - **[GPT——Generative pre-trained transformer](https://en.wikipedia.org/wiki/Generative_pre-trained_transformer)**
        - 基于转换器的生成式预训练模型是一种大型语言模型(LLM)，也是生成式人工智能的重要框架。首个GPT由OpenAI于2018年推出。GPT模型是基于Transformer模型的人工神经网络，在大型未标记文本数据集上进行预训练，并能够生成类似于人类自然语言的文本。截至2023年，大多数LLM都具备这些特征，并广泛被称为GPT。
- ChatGPT相比于之前的模型有什么技术优势?
    - 模型规模更大:ChatGPT采用了更多的参数和更深的网络结构,从而使得模型更加精细和复杂,在生成对话时可以提供更加准确、人性化、自然的回复。
    - 训练数据更广泛:ChatGPT使用了大规模、多样的文本数据作为训练数据,包括新闻报道、电影剧本、百科全书、社交媒体等不同类型的文本,从而使得模型能够涵盖更丰富、更广泛的语言特性和语境背景,提高了模型的效果。
    - 更高效的模型与训练:ChatGPT采用了Transformer技术,使得模型在预训练时可以更好地捕捉到语法、语义、上下文等信息,从而在生成对话时具有更高的准确性和流畅程度。
    - 支持动态生成:ChatGPT 支持在运行时根据上下文内容动态生成回复，从而可以更好地适应不同场景和用户需求，提高了模型的灵活性和可定制性。
### AI Agent的定义
- **[AI Agent](https://en.wikipedia.org/wiki/Intelligent_agent)**
    - 在人工智能领域中,Agent是指:以智能方式行事的代理,它感知环境,自主采取行动以实现目标,并可以通过学习或获取知识来提升其性能。
![AI Agent流程图](https://github.com/ShanHaiAI/KuaFu/blob/main/2_应用开发/1_Agent/workflow/IntelligentAgent-SimpleReflex.png)
### [为什么LLM可以实现Agent?](https://www.zhihu.com/tardis/zm/art/629087587?source_id=1003)
  如果问题相对简单，不需要什么逻辑推理，可能靠大模型背答案就能做得不错，但是对于一些需要推理的问题，都不用太难，就一些简单的算术应用题，大模型就大概率不太 work。于是，思维链（Chain-of-Thought，CoT）很自然地被提出了。
    
  [思维链](https://arxiv.org/abs/2201.11903)的主要思想:通过向大语言模型展示一些少量的example,在样例中解释推理过程,大语言模型在回答提示时也会显示推理过程。
  
  思维链提示会引发大语言模型的推理,使得大模型的逻辑推理能力与之前相比，完全不一样了,具体有三个不一样:
  - 常识推理能力赶超人类
  - 数学逻辑推理大幅提升
  - 大语言模型更具可解释性,更加可信

## 应用:怎么实现Agent?
### **[论文:ReAct](https://arxiv.org/abs/2210.03629)**
- ReAct:Synergizing Reasoning and Acting in Language Models
  - 探索使用LLM以交错的方式生成推理轨迹和特定于任务的动作,从而实现两者之间更大的协同作用:推理轨迹可帮助模型归纳、跟踪和更新行动计划以及处理异常,而操作允许它与外部源交互,以收集附加信息。
  - ReAct = Reasoning + Acting
    - 通过引导模型以自问自答的形式来实现逻辑和执行交错循环,通过逻辑(Reasoning)来帮助模型决定下一步需要做什么(调用什么样的工具解决问题),再通过执行(Acting)来获得新的信息,从而帮助模型进行进一步决策,循环直到问题解决并生成最终答案。
  - CoT和ReAct的区别
    - CoT是解决问题的过程,通过分解问题减少模型出现推理错误的可能。
    - ReAct是结合了推理和执行的步骤,让模型根据执行结果进一步进行推理。
### 通过[LangChain](https://python.langchain.com/docs/get_started/introduction.html)实现Agent
- LangChain
  - LangChain是一个用于开发由语言模型支持的应用程序的框架。它支持一下应用程序:
    - 数据感知:将语言模型连接到其他数据源
    - Agentic:允许语言模型与其环境交互
  - LangChain的主要价值:
    - 组件:用于处理语言模型的抽象,以及每个抽象的实现集合。无论您是否使用LangChain框架的其余部分,组件都是模块化且易于使用的
    - 现成的链:用于完成特定更高级别任务的组件的结构化组装。
- LangChain实现Agent
  - Agent的核心思想是使用语言模型来选择要采取的一系列措施.在Agent中,语言模型被用作推理引擎来确定要采取哪些操作以及按什么顺序。
![LangChain实现Agent](https://github.com/ShanHaiAI/KuaFu/blob/main/2_应用开发/1_Agent/workflow/langchain.png)
### **[应用产品:MetaGPT](https://github.com/geekan/MetaGPT)**

## 延申:如何去优化Agent
### 通过微调使LLM更适应Agent任务
- **[AgentTuning:Enabling Generalized Agent Abilities For LLMs](https://arxiv.org/abs/2310.12823)**
  - [AgentTuning](https://github.com/THUDM/AgentTuning)是首次利用多个Agent任务交互轨迹对LLM进行指令调整的方法。评估结果表面,AgentTuning让LLM在未见过的Agent任务中也展现出强大的泛化能力,同时通过语言能力也基本保持不变。
![AgentTuning](https://github.com/ShanHaiAI/KuaFu/blob/main/2_应用开发/1_Agent/workflow/tuning.png)

### [未来前景](https://mp.weixin.qq.com/s/QwkTp1N5DMGtXxi56nTBew)
- 吴恩达教授指出:AI Agent workflow将推动人工智能取得巨大进步,甚至可能超过下一代基础模型。
![吴恩达教授的观点](https://github.com/ShanHaiAI/KuaFu/blob/main/2_应用开发/1_Agent/workflow/future_agent.png)

- 框架内容:
  - reflection:LLM检查自己的工作,以提出改进方式。
    - 提示词引导:借助提示词工程和上下文记忆,让模型自己修改输出内容,从而达到完善输出内容的功能。
  - 工具使用:LLM拥有网络搜索、代码执行或任何其他功能来帮助其收集信息、采取行动或处理数据。
  - 规划:LLM提出并执行一个多步骤计划来实现目标(例如:撰写论文大纲,然后进行在线研究,然后撰写草稿...)
  - 多智能体协作:多个AI Agent一起工作,分配任务并讨论和辩论想法,以提出比单个Agent更好的解决方案。
    - [阿里魔搭:AgentScope](https://modelscope.github.io/agentscope/zh_CN/index.html)
    - [SuperAGI框架实现的agent](https://ishaan-bhola.medium.com/building-autonomous-business-processes-using-ai-agent-workflows-216c30ae0ae1)


