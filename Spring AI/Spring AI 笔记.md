

[飞书笔记](https://my.feishu.cn/wiki/NQEYwdxDkiUvaJkMwiicHDVqnug)

# 大模型应用开发——模型部署方案对比


### 一、开放 API 方案（最常用、最快速）

通过直接调用第三方大模型厂商（如 OpenAI、阿里云百炼、DeepSeek 等）提供的标准接口来构建应用。

  

- **核心优势**
    
      
    - **前期成本极低**：采用按需付费（Token 计费）模式，完全省去了前期数万甚至数十万的硬件投入。
        
          
        
    - **无需部署**：省去了复杂的模型加载、显卡驱动配置和推理优化环节。
        
          
        
    - **无需维护**：底层的模型升级、故障排查和服务器监控全部由云厂商负责。
        
          
        
    - **全球访问**：依托大厂成熟的全球骨干网，能够提供极低延迟和高并发保障。
        
          
        
- **主要劣势**
    
      
    - **数据隐私**：企业的核心业务数据、用户隐私需要传输到第三方服务器，存在数据泄露与合规风险。
        
          
        
    - **网络依赖**：应用系统必须在稳定且高速的互联网环境下才能运行。
        
          
        
    - **长期成本高**：当业务进入爆发期、用户量和调用量呈指数级增长时，长期的 API 费用会非常惊人。
        
          
        
    - **定制限制**：无法对大模型的底层架构进行深度的二次开发或私有化微调。
        
          
        
- **Spring AI 开发建议**：
    
      
    - 在 Spring AI 中，这是最容易实现的方案。你只需要引入对应的 Starter（例如 `spring-ai-openai-spring-boot-starter`），并在 `application.yml` 中配置好 API Key 即可快速开始编写 `ChatClient` 代码。
        
          
        

### 二、本地部署方案（私有化、高安全）

将开源大模型（如 Llama 3、Qwen、Mistral 等）下载并部署在企业自有或租赁的 GPU 服务器上（通常借助 Ollama、vLLM 等推理框架）。

  

- **核心优势**
    
      
    - **数据安全**：所有数据和推理过程完全在本地或私有内网闭环，绝对自主可控，完美契合金融、医疗、政务等行业的强合规要求。
        
          
        
    - **不依赖外部网络**：即使在完全断网或局域网的独立物理环境中也能正常工作。
        
          
        
    - **长期成本低**：在海量调用、超高并发的长期业务场景下，均摊昂贵的硬件购置成本后，综合性价比往往超越 API 方案。
        
          
        
    - **高度定制**：支持对模型进行自由的微调（Fine-tuning）、量化（Quantization）以及架构层面的深度改造。
        
          
        
- **主要劣势**
    
      
    - **初始成本高**：需要一次性采购高昂的专业 AI 服务器及高显存 GPU 硬件（如 NVIDIA A100/H100/RTX 4090 等）。
        
          
        
    - **维护复杂**：需要专业的 AI 运维团队来处理显卡驱动、CUDA 版本冲突、推理加速和集群监控。
        
          
        
    - **部署周期长**：从硬件采购、机房上架、环境搭建到模型调优，整体落地周期较久。
        
          
        
- **Spring AI 开发建议**：
    
      
    - Spring AI 完美支持本地推理。例如配合 **Ollama**，你只需引入 `spring-ai-ollama-spring-boot-starter`，Spring AI 就能无缝对接你本地运行的大模型，代码层面的体验与调用云端 API 几乎完全一致。
        
          
        

### 三、云部署方案（弹性、托管式）

将自己微调后的模型或开源大模型托管到云服务商（如 AWS Bedrock、阿里云 PAI 等）的容器或虚拟机上。

  

- **核心优势**
    
      
    - **前期成本低**：无需企业自建机房，按需租赁云端 GPU 实例，降低了初期门槛。
        
          
        
    - **部署维护简单**：云厂商提供一站式容器化工具和监控面板，大幅减轻了运维压力。
        
          
        
    - **弹性扩展**：可以根据业务流量的波峰波谷，实现自动扩缩容，灵活分配算力。
        
          
        
    - **全球访问**：借助云服务商的全球节点，方便面向全球用户提供服务。
        
          
        
- **主要劣势**
    
      
    - **数据隐私**：虽然属于私有模型实例，但底层基础设施仍托管在第三方云端，仍需评估云厂商的隐私协议。
        
          
        
    - **网络依赖**：依然高度依赖稳定的网络连接。
        
          
        
    - **长期成本高**：长期租赁高配云端 GPU 的累积费用同样十分可观。
        
          
        




# 大模型应用开发——本地部署

在大模型应用开发中，本地部署（私有化部署）是许多企业和开发者为了追求数据安全、合规可控以及长期成本优化而选择的重要方案。结合你正在学习的 **Spring AI**，本地部署不仅改变了底层基础设施，也直接决定了你在 Spring Boot 项目中的技术选型。


## 一、本地部署的核心价值与应用场景

- **数据绝对安全与隐私合规**：所有数据和推理过程完全在本地或私有内网闭环，无需将敏感业务数据上传至第三方云端，完美契合金融、医疗、政务等行业的强合规要求。
    
      
    
- **离线稳定运行**：不依赖外部互联网连接，即使在完全断网或局域网的独立物理环境中也能保持高可用。
    
      
    
- **自由定制与微调**：支持对开源大模型（如 Llama、Qwen 等）进行自由的量化、微调或架构改造，满足特定业务场景的深度需求。
    
      
    

## 二、主流本地部署工具对比（Ollama vs vLLM）

在进行本地部署时，开发者通常会根据所处阶段选择不同的开源推理工具：

  

- **Ollama（适合本地开发、原型验证与学习）**
    
      
    - **特点**：以极简体验著称，支持“一行命令跑模型”，对新手极为友好。
        
          
        
    - **适用场景**：个人电脑本地调试、Spring AI 快速上手、轻量级原型验证。
        
          
        
- **vLLM（适合企业级生产与高并发场景）**
    
      
    - **特点**：创新性地引入了 PagedAttention 内存管理算法，大幅提升了 GPU 显存利用率、吞吐量和多用户并发处理能力。
        
          
        
    - **适用场景**：生产环境、高并发在线智能客服、多卡集群部署。
        
          
        

## 三、Spring AI 如何无缝对接本地部署

Spring AI 对本地部署提供了非常出色的原生支持，让 Java 开发者能够像调用云端 API 一样轻松调用本地模型：

*spring-ai-ollama 依赖是 Java 客户端包，只是发 http 请求的 SDK，本身不带 Ollama 服务和模型，必须有一个可访问的 Ollama 服务实例，Ollama 是独立后端服务。 只加依赖、不启动 Ollama 服务，启动项目直接连不上、报错。需要本地安装 Ollama，启动后默认 `http://127.0.0.1:11434`，Spring AI 连这个地址*


1. **集成 Ollama Starter**：
    
    在 Spring Boot 项目中引入 `spring-ai-ollama-spring-boot-starter` 依赖。
    
      
    
2. **配置文件连接**：
    
    在 `application.yml` 中指定本地推理服务的地址：
    
      
    
    ```YAML
    spring:
      ai:
        ollama:
          base-url: http://localhost:11434
          chat:
            options:
              model: qwen2.5 # 指定你本地下载的模型
    ```
    
3. **编写业务代码**：
    
    直接注入 `ChatClient` 即可实现本地大模型的文本生成、流式响应或 RAG 检索增强生成，代码层面的迁移成本极低。
    
      
    

## Ollama 介绍


**Ollama** 是目前最流行的大模型本地运行与轻量级管理工具。它将复杂的 LLM 推理引擎（基于 llama.cpp）封装成了类似于 Docker 的极简命令行界面，并提供开箱即用的 RESTful API 接口，是 Java 开发者结合 **Spring AI** 进行本地开发、原型设计和私有化部署的首选方案。

  

### 一、Ollama 的核心优势

1. **开箱即用（Docker 式体验）**：使用简单的命令（如 `ollama run llama3`）即可完成大模型的下载、加载与交互。
    
      
    
2. **轻量与资源优化**：内置 CPU/GPU 混合推理加速与自动量化支持（如 4-bit 量化），极大降低了本地显存和内存占用。
    
      
    
3. **标准化 REST API**：默认在 `http://localhost:11434` 提供与 OpenAI 协议兼容或原生的 REST 接口，非常适合后端集成。
    
      
    
4. **丰富的模型生态**：支持目前主流的开源大模型，如 DeepSeek-R1、Qwen2.5、Llama 3、Mistral 等。
    
      
    

### 二、常用 CLI 指令速查

- **拉取并运行模型**：`ollama run <model_name>`（如 `ollama run deepseek-r1:7b` 或 `ollama run qwen2.5`）
    
      
    
- **仅下载模型**：`ollama pull <model_name>`
    
      
    
- **查看已下载模型**：`ollama list`
    
      
    
- **删除模型**：`ollama rm <model_name>`
    
      
    
- **查看服务运行状态**：`ollama ps`
    
      
    

### 三、Spring AI 集成 Ollama 实战步骤

在 Spring Boot 项目中，只需三步即可完成 Spring AI 与本地 Ollama 的无缝对接：

  

#### 1. 引入 Maven 依赖

在项目的 `pom.xml` 中引入 Spring AI Ollama Starter：

  

```XML
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-ollama-spring-boot-starter</artifactId>
</dependency>
```

#### 2. 配置 `application.yml`

配置 Ollama 的服务地址以及要调用的本地模型名称：


```YAML
spring:
  ai:
    ollama:
      base-url: http://localhost:11434 # Ollama 默认监听地址
      chat:
        options:
          model: qwen2.5                 # 你本地下载的模型名称
          temperature: 0.7              # 采样温度
```

#### 3. 编写 Controller 控制层代码

利用 Spring AI 提供的 `ChatClient` 实现文本生成或流式输出（Streaming）：


```Java
@RestController
@RequestMapping("/ai")
public class ChatController {

    private final ChatClient chatClient;

    public ChatController(ChatClient.Builder builder) {
        this.chatClient = builder.build();
    }

    // 普通文本生成
    @GetMapping("/chat")
    public String chat(@RequestParam(value = "message") String message) {
        return chatClient.prompt()
                .user(message)
                .call()
                .content();
    }

    // SSE 流式响应（打字机效果）
    @GetMapping(value = "/stream", produces = MediaType.TEXT_EVENT_STREAM_VALUE)
    public Flux<String> streamChat(@RequestParam(value = "message") String message) {
        return chatClient.prompt()
                .user(message)
                .stream()
                .content();
    }
}
```

### 四、高级特性与企业扩展

- **Tool Calling / Function Calling（函数调用）**：Spring AI 的 Ollama 模块原生支持将 Java 方法注册为 Bean 并传给本地大模型，让本地大模型能够实时调用本地系统接口或查询数据库。
    
      
    
- **Embedding 与 RAG**：Ollama 支持部署嵌入模型（如 `nomic-embed-text`），结合 Spring AI 的 VectorStore（如 PGVector、Milvus），可直接在本地搭建一套完全离线的私有知识库 RAG 系统。
    
      
    

**Ollama** 是目前最流行的开源大模型（LLM）本地运行工具。它的设计理念类似于 **Docker**，把大模型的下载、量化、运行以及 API 服务封装得极为简单，让开发者可以在普通 PC 或服务器上“一行命令”跑起大模型。



## Ollama 使用指南


### 一、 安装 Ollama

1. **Windows / macOS**：
    
      
    - 前往官网 [ollama.com](https://ollama.com/) 下载对应的安装包，直接双击安装即可。
        
          
        
    - 安装完成后，Ollama 会在后台作为系统服务自动运行。
        
          
        
2. **Linux**：
    
      
    - 在终端执行官方安装脚本：
        
          
        
        ```Bash
        curl -fsSL https://ollama.com/install.sh | sh
        ```
        

### 二、 核心命令与日常使用

Ollama 的使用体验极具 Docker 风格，常用命令如下：


#### 1. 运行并与模型对话 (`ollama run`)

如果本地没有该模型，Ollama 会自动从云端拉取（Pull）并直接启动终端交互终端：


```Bash
# 运行通义千问 2.5（7B 参数版本）
ollama run qwen2.5

# 运行 DeepSeek 深度推理模型
ollama run deepseek-r1:7b

# 运行 Meta 的 Llama 3
ollama run llama3
```

进入交互界面后：

  

- 输入你的问题按回车即可获得回答。
    
      
    
- 输入 `/?` 查看快捷指令。
    
      
    
- 输入 `/exit` 或按下 `Ctrl + D` 退出对话。
    
      
    

#### 2. 仅下载模型到本地 (`ollama pull`)

如果不想立即对话，仅下载模型：


```Bash
ollama pull qwen2.5
```

#### 3. 查看本地已安装的模型列表 (`ollama list`)


```Bash
ollama list
```

输出会展示模型名称、标签、文件大小及修改时间。


#### 4. 查看当前运行中的模型 (`ollama ps`)


```Bash
ollama ps
```

显示当前已加载进内存/显存中的模型，以及其占用的资源状态。

  

#### 5. 删除本地模型 (`ollama rm`)


```Bash
ollama rm qwen2.5
```

### 三、 HTTP API 服务（开发者调用）

Ollama 启动后，默认会在本地后台监听 **`http://localhost:11434`**，并对外提供开箱即用的 RESTful API 接口。

  
#### 1. 标准原生 API 示例 (`/api/generate`)

可以通过 `curl` 或 Postman 发送 POST 请求：

  

```Bash
curl http://localhost:11434/api/generate -d '{
  "model": "qwen2.5",
  "prompt": "用一句话介绍你自己",
  "stream": false
}'
```

#### 2. 兼容 OpenAI 格式的 API 示例 (`/v1/chat/completions`)

Ollama 原生兼容 OpenAI 的 API 结构，这使得绝大多数支持 OpenAI 的第三方客户端或 SDK 都能直接对接它：

  
```Bash
curl http://localhost:11434/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "qwen2.5",
    "messages": [
      {"role": "user", "content": "你好！"}
    ]
  }'
```

### 四、 自定义与扩展使用

#### 1. 修改模型系统提示词（System Prompt）或创建自定义模型

类似于 Dockerfile，Ollama 支持通过 **`Modelfile`** 来自定义模型行为。

  

1. 新建一个名为 `Modelfile` 的文件，内容如下：
    
      
    
    ```Dockerfile
    FROM qwen2.5
    
    # 设置系统提示词
    SYSTEM "你是一个严谨且极具幽默感的 Java 架构师，回答问题喜欢举生动的比喻。"
    
    # 设置采样温度（控制随机性）
    PARAMETER temperature 0.3
    ```
    
2. 构建并生成你的新模型：
    
      
    
    ```Bash
    ollama create my-java-expert -f ./Modelfile
    ```
    
3. 运行你的定制模型：
    
      
    ```Bash
    ollama run my-java-expert
    ```
    

#### 2. 修改默认模型存储路径（环境变量设置）

大模型文件较大（动辄 4G~20G），如果 C 盘空间不足，可以通过环境变量指定模型保存位置：

  

- **Windows**：在系统环境变量中新建 `OLLAMA_MODELS`，值为你的路径（如 `D:\ollama_models`）。
    
      
    
- **Linux / macOS**：在环境变量中设置 `export OLLAMA_MODELS="/path/to/models"`。
    
      
    

掌握以上命令和 API 后，基本上就覆盖了 Ollama 在本地开发与日常测试中的绝大多数场景。如果想尝试配合桌面 WebUI 工具（如 Open WebUI）使用，也可以很方便地直接连接本地的 `11434` 端口。



# 大模型应用开发——云部署

在大模型应用开发中，**云部署（Cloud Deployment）** 是介于“调用第三方开放 API”与“完全自建本地服务器”之间的一种折中且主流的架构方案。它指的是将开源或微调后的大模型托管在云服务商（如阿里云 PAI、AWS Bedrock、腾讯云、UCloud 等）的云端虚拟机或容器实例上进行运行和管理。



## 一、云部署的核心价值与应用场景

- **前期成本适中**：企业无需一次性采购数万元甚至数十万元的高昂 GPU 硬件（如 A100/H100），而是通过按需租赁云端 GPU 实例来降低冷启动门槛。
    
      
    
- **部署与运维简化**：主流云厂商通常会提供一站式的容器化工具、镜像模板以及监控面板，有效减轻了底层驱动、CUDA 配置等繁琐的运维负担。
    
      
    
- **弹性扩展能力**：支持根据业务流量的波峰波谷进行动态的自动扩缩容，灵活应对突发的高并发访问。
    
      
    
- **全球网络加速**：依托云服务商成熟的全球骨干网和边缘节点，能够为异地或全球用户提供低延迟的访问体验。
    
      
    

## 二、云部署的主要挑战与劣势

- **数据隐私与合规风险**：尽管模型是独占部署在云端，但底层硬件和基础设施依然托管在第三方云厂商手中，对于金融、医疗等极度敏感的行业，仍需严格评估云服务商的合规协议。
    
      
    
- **网络依赖性强**：应用程序与云端大模型实例之间必须保持稳定、高速的互联网连接。
    
      
    
- **长期成本不可忽视**：如果业务处于7x24小时的高负载运行状态，长期租赁高配云端 GPU 的累积费用往往非常高昂。
    
      
    

## 三、Spring AI 如何对接云部署方案

在 Spring AI 中，云部署的对接方式通常取决于你在云端选择的推理框架：

  

1. **兼容 OpenAI API 协议的云端托管**：
    
    许多云厂商（或托管平台）在部署开源模型（如 Llama 3、Qwen）时，会提供兼容 OpenAI 格式的 RESTful API 接口。此时，你在 Spring AI 中依然可以使用 `spring-ai-openai-spring-boot-starter`，只需简单修改 `base-url` 指向你的云端服务地址即可：
    
      
    
    ```YAML
    spring:
      ai:
        openai:
          base-url: https://your-cloud-gpu-endpoint.com # 云端托管模型的访问地址
          api-key: your-cloud-api-key
          chat:
            options:
              model: qwen2.5
    ```
    
2. **专属云服务集成**：
    
    如果使用的是各大云厂商原生的 AI 平台（如 AWS Bedrock 等），Spring AI 也提供了专门的 Starter 依赖（如 `spring-ai-bedrock-spring-boot-starter`），让开发者能够原生调用云厂商的托管模型服务。
    
      
    




# 大模型应用开发——调用大模型（API）


![[Pasted image 20261012180726.png]]


### 一、 代码整体格式（OpenAI 标准格式）

这段代码采用了大模型行业最通用的 **OpenAI SDK 兼容格式**。

仔细观察代码你会发现：DeepSeek 虽然是独立的厂商，但它的 SDK 导入和调用方法与 **OpenAI 完全一致**。

- 这是目前大模型行业的通用标准（OpenAI API 格式规范）。
    
- 绝大多数大模型厂商（如 DeepSeek、通义千问、智谱等）为了降低开发者的迁移成本，都提供了兼容 OpenAI 格式的接口。

整体分为三个标准步骤：


1. **客户端初始化**：导入 SDK 并配置密钥与网关地址。
    
      
    
2. **发起请求（`chat.completions.create`）**：传入模型名称、对话历史和控制参数。
    
      
    
3. **解析返回结果**：从嵌套的 JSON 对象中提取大模型生成的文本内容。
    
      
    

### 二、 核心参数详细介绍

在 `client.chat.completions.create()` 方法中，包含了几个至关重要的参数：

  

- **`model`**
    
      
    - **含义**：指定本次请求要调用的具体大模型版本。
        
          
        
    - **示例**：代码中填入的是 `"deepseek-r1"`（代表 DeepSeek 的深度推理模型）。
        
          
        
- **`messages`（核心对话数组）**
    
      
    - **含义**：用于传递整个对话的上下文历史，它是一个由多个字典（消息对象）组成的列表。
        
          
        
    - **角色（`role`）分类**：
        
          
        - **`system`（系统角色）**：用于为大模型设定“人设”、行为规范或全局背景规则。代码中设定为：`"你是一个热情的AI助手，你的名字叫小团团"`。
            
              
            
        - **`user`（用户角色）**：代表人类用户向模型提出的实际问题或指令。代码中为：`"你好，你是谁？"`。
            
              
            
        - _(注：标准的 OpenAI 格式中还包含 `assistant` 角色，用于存放模型之前的历史回答)_
            
              
            
- **`stream`**
    
      
    - **含义**：控制响应模式的布尔值。
        
          
        
    - **取值**：
        
          
        - `False`：**非流式模式**。大模型会在后台把整段话生成完毕后，一次性将完整结果返回给前端。
            
              
            
        - `True`：**流式模式（Stream）**。大模型会像打字机一样，逐字逐句实时返回数据（常用于实现网页端的流式输出效果）。
            
              
            

### 三、 初始化与响应解析参数

- **`api_key`**
    
      
    - **含义**：你在大模型厂商（如 DeepSeek 平台）注册账号后申请的身份鉴权密钥，用于计费和权限校验。
        
          
        
- **`base_url`**
    
      
    - **含义**：API 服务的底层网关地址。因为 DeepSeek 拥有独立的服务器集群，所以需要将该地址显式指定为 `[https://api.deepseek.com](https://api.deepseek.com)`。
        
          
        
- **`response.choices[0].message.content`**
    
      
    - **含义**：这是解析大模型返回结果的标准路径。
        
          
        
    - `choices` 是一个列表（支持一次性返回多个候选回答），通常取第一个候选 `[0]`；`message.content` 则是大模型最终输出的文本正文。


### 补充：云部署和调用大模型区别

|**比较维度**|**调用开放 API**|**云部署（自建托管）**|
|---|---|---|
|**谁来运行模型？**|厂商的超级机房|你在云端租用的独立 GPU 虚拟机/容器|
|**计费方式**|**按量付费**（根据你消耗的 Token 数量计费，用多少付多少）|**按时租赁**（不论有没有人跟你聊天，只要 GPU 开机，就要按小时/按月付服务器租金）|
|**数据隐私**|数据会发送到第三方厂商服务器，存在合规或泄露风险|数据完全留在你自己的云端私有环境里，不出内网，绝对安全|
|**定制与微调**|无法修改模型底层，只能用厂商预设的模型|可以对模型进行自由的**微调（Fine-tuning）**、量化或更换推理框架|
|**运维负担**|零运维。厂商负责升级、抗并发、修复 Bug|繁重运维。你需要自己处理显卡驱动、CUDA 版本、并发崩溃、模型升级|


# 大模型应用开发——什么是大模型应用


### 一、 传统程序 vs AI大模型能力对比


- **大模型擅长的领域（传统程序搞不定的）**
    
      
    - **模糊问题处理**：例如写一首关于秋天的诗、开放式问答、文案润色。
        
          
        
    - **复杂模式识别**：例如判断一张图片是“猫”还是“狗”、情感分析、语义理解。
        
          
        
- **传统程序擅长的领域（大模型的弱项）**
    
      
    - **确定性逻辑处理**：例如精确的数学计算、数据库事务、订单状态机校验（大模型算数学题经常出错）。
        
          
        
    - **精确控制和高可靠性**：例如内存管理、高并发安全控制、底层系统调用。
        
          
        

**💡 什么是大模型应用？** “大模型应用是基于大模型的推理、分析、生成能力，结合传统编程能力，开发出的各种应用。” 在 Spring AI 中，你就是那个程序员，用 Java 代码（传统程序）去控制流程、连接数据库、做校验，同时把复杂、智能的思考部分交个大模型去处理。

  

### 二、 主流大模型与对话产品全景


- **国际顶尖**
    
      
    - **OpenAI (GPT-3.5 / GPT-4o)**：对应产品为 **ChatGPT**，全球大模型领域的开拓者和标杆。
        
          
        
    - **Anthropic (Claude 3.5)**：对应产品为 **Claude AI**，在代码编写和长文本处理上表现极其出色。
        
          
        
- **国内领先**
    
      
    - **DeepSeek (DeepSeek-R1)**：对应产品为 **DeepSeek**，凭借极高的性价比和出色的深度推理能力（R1）在全球爆火。
        
          
        
    - **阿里巴巴 (Qwen-Max)**：对应产品为 **通义千问**，对中文理解深度极佳，且在开源生态（如 Qwen2.5）上对开发者非常友好。
        
          
        
    - **百度 (文心大模型 3.5)**：对应产品为 **文心一言**。
        
          
        
    - **月之暗面 (Moonshot)**：对应产品为 **Kimi**，主打超长上下文处理。
        
          
        
    - **科大讯飞 (星火 3.5)**：对应产品为 **讯飞星火**。
        
          
        
    - **零一万物 (Yi-Large)**：由李开复博士创立的大模型公司。
        
          
        

在实际开发中，**Spring AI** 的最大魅力就在于它提供了一套**统一的抽象接口（ChatClient）**。无论你后面想切换成 DeepSeek、通义千问还是 OpenAI，底层的 Java 业务代码几乎不用大改，只需要更换 Starter 或配置的 `base-url` 即可！


# 大模型应用开发——AI 应用开发技术架构

![[Pasted image 20261012183750.png]]


## 一、 AI 应用开发技术架构全景：PAFR 模型

在大模型应用开发中，通常会围绕四个核心技术支柱来构建系统：


1. **Prompt（纯 Prompt 问答）**：利用大模型的自身推理能力，通过精心编排的提示词来完成特定的业务逻辑。
    
      
    
2. **Agent + Function Calling**：让 AI 具备任务拆解与工具调用的能力，由大模型自主决定调用业务端提供的接口来解决复杂业务。
    
      
    
3. **Fine-tuning（模型微调）**：针对特定的业务场景和领域数据，对基础大模型进行深度的数据训练与权重微调。
    
      
    
4. **RAG（Retrieval-Augmented Generation，检索增强生成）**：为大模型外挂私有知识库，让大模型基于检索到的外部知识片段进行精准推理和回答。
    
      
    

## 二、 四大核心架构的工作原理与时序流程

### 1. 纯 Prompt 问答

- **核心特征**：完全依赖大模型原生的推理与生成能力来支撑应用的核心功能。
    
- **典型应用场景**：文本摘要分析、舆情分析、坐席检查、通用 AI 对话。
    
- **交互流程**：
      
    1. 用户向 AI 应用提出问题。
        
    2. AI 应用在本地组织并组装对应的 Prompt 模板。
        
    3. AI 应用将组装好的 Prompt 发送给基础大模型。
        
    4. 大模型生成结果并返回响应，最终由 AI 应用呈现给用户。
        

### 2. Function Calling（函数调用 / Agent 基础）

- **核心特征**：将应用程序端的业务能力（如查天气、查数据库、调用第三方接口）与大模型的推理能力无缝结合，极大地简化了复杂业务功能的开发。
    
- **典型应用场景**：智能旅行指南、企业数据提取、数据聚合分析、智能课程顾问。
    
- **交互流程**：
      
    1. 用户向 AI 应用提交业务诉求。
        
    2. AI 应用组织包含系统提示词及可用函数（Function）定义的 Prompt 发送给大模型。
        
    3. 大模型在内部进行任务拆解，自主判断是否需要调用某个 Function。
        
    4. 若需要调用，大模型会返回需要调用的函数名称及对应参数。
        
    5. AI 应用在本地执行该函数，并将执行结果拼接到 Prompt 中再次发送给大模型。
        
    6. 循环往复直到任务完成，大模型输出最终的业务响应结果。
        

### 3. RAG（检索增强生成）与 Embeddings

- **核心特征**：通过“外挂知识库”的方式，解决大模型知识滞后、产生幻觉以及无法访问企业内部私有数据的问题。
    
- **典型应用场景**：企业内部私有知识库问答、智能客服助手。
    
- **核心步骤**：
      
    - **离线步骤**：① 文档加载 $\rightarrow$ ② 文档切分 $\rightarrow$ ③ 文档向量化编码（Embeddings） $\rightarrow$ ④ 写入向量知识库。
        
    - **在线交互步骤**：
          
        1. 获取用户的提问。
            
        2. 在知识库中进行向量检索，匹配出与问题高度相关的知识片段。
            
        3. 将检索到的知识片段与用户的原始问题一同填入 Prompt 模板中。
            
        4. 用组装好的完整 Prompt 调用大模型（LLM）。
            
        5. 由 LLM 基于外挂知识库的内容生成准确的回复并返回给用户。


### 4. Fine-tuning（微调）*了解*


在实际的 AI 应用开发中，**“大模型应用开发很少用到微调（Fine-tuning）”**。在 90% 的业务场景下，开发者更倾向于使用 **Prompt、RAG 和 Agent**，而不是一上来就去微调模型。

#### 一、 什么是微调（Fine-tuning）？

- **本质**：微调是拿着大量的“领域专属数据集”（比如数万条问答对、代码、行业文档），去对开源的基础大模型进行**二次深度训练**，通过改变模型的内部权重参数，让它“学会”某种特定的专业技能、说话语气或固定格式。
    
      
    
- **通俗比喻**：如果把大模型比作一个刚毕业的大学生，微调就像是送他去参加一个为期半年的“医学专科定向培训班”，让他在特定领域变得更加专业。
    
      
    

#### 二、为什么 AI 应用开发大都用不上微调

在企业级 AI 应用开发中，微调往往不是首选，原因主要有以下四点：


1. **成本极其高昂**：
    
    微调需要准备高质量的训练集（清洗数据非常痛苦），还需要消耗高昂的 GPU 算力和时间成本。
    
2. **知识无法实时更新（灾难性的“知识滞后”）**：
    
    微调把知识“写死”在了模型的权重里。如果你们公司的产品价格明天变了，或者出了新规，微调模型是不知道的，**你必须重新花钱、花时间去训练一个新版本**。而 RAG（知识库）只需要在数据库里修改一条数据就搞定了。
    
3. **基础模型已经足够强大**：
    
    现在的基座模型（如 DeepSeek-R1、Qwen2.5 等）通用能力极其强悍。通过优秀的 **Prompt（提示词工程）** 和 **RAG（外挂知识库）**，它们就能完美胜任绝大多数业务需求。
    
    
4. **容易产生“灾难性遗忘”**：
    
    有时候为了让模型学会某个新领域专门去微调，结果发现它原本很聪明的逻辑推理能力反而下降了。
     
    

#### 三、 那什么情况下，AI 应用开发才**真的需要**微调？

虽然大部分应用不需要，但在以下特定场景下，微调依然是不可替代的：


- **极其严格的格式控制**：比如要求大模型每次输出必须严格符合某个极其复杂的旧系统 JSON 规范，微调可以大幅降低格式出错率。
	
	
- **独特的品牌人格与语气（Persona）**：比如做一个虚拟偶像或特定客服，需要它拥有非常独特、甚至带口癖的说话风格，光靠 Prompt 很难完美模拟时。
      
    
- **小众的垂直领域/冷门语言**：如果你们的业务涉及某种极其小众的行业黑话、古代方言或罕见的编程语言，基座模型没学过，就必须通过微调来“恶补”。
      
    

### 总结

在学习 **Spring AI** 和构建应用时，你应该把核心精力放在 **Prompt 编排、RAG 知识库搭建（VectorStore）以及 Function Calling（Agent）** 上。微调通常是 AI 科学家或大模型厂商做的事情，普通应用开发者更像是“架构师”和“拼图者”！



# Spring AI 入门介绍


## 一、 什么是 Spring AI？

Spring AI 是 Spring 官方推出的 AI 工程应用框架，旨在将 Spring 生态系统一贯的“可移植性”、“依赖注入”和“模块化设计”理念引入大模型应用开发领域。它为 Java 开发者提供了一套标准、统一的抽象层，彻底屏蔽了底层不同大模型厂商（如 OpenAI、DeepSeek、Ollama 等）在 API 接口上的差异。

  

## 二、 Spring AI 的核心设计理念

- **统一接口抽象**：通过 `ChatClient`、`VectorStore` 等标准组件，实现“一套代码，随处切换模型”，更换大模型时只需调整配置而无需重构业务逻辑。
    
      
    
- **POJO 编程模型**：完美融合 Spring Boot 的自动配置与生命周期管理，让开发者像编写传统的 Java 后端服务一样构建 AI 应用。
    
      
    
- **企业级生态原生集成**：无缝对接 Spring 现有的安全、事务、依赖注入及监控体系，适合构建规范、稳健的生产级后端系统。
    

## 三、 Spring AI 与其他主流 AI 开发技术的对比

| **维度**      | **Spring AI (Java 体系)**                      | **Python LangChain / LlamaIndex**   | **厂商原生 SDK / 直接 HTTP 调用**        |
| ----------- | -------------------------------------------- | ----------------------------------- | -------------------------------- |
| **技术栈生态**   | 专为 Java / Spring Boot 开发者打造，完美融入企业现有后端架构     | Python 社区的主流 AI 编排框架，AI 算法与研究员首选    | 依赖特定厂商（如 OpenAI、DeepSeek）的底层接口规范 |
| **企业系统集成**  | **极高**。天然支持 Spring Security、微服务、企业级数据库与权限管控  | **中等**。通常需要额外通过 Python 编写网关或微服务进行封装 | **较低**。代码与特定厂商强耦合，未来若需切换模型迁移成本极高 |
| **功能丰富度**   | 持续快速迭代中，涵盖 Chat、RAG、Function Calling 等核心企业刚需 | 生态极其庞大繁荣，拥有全行业最丰富的 Agent 编排与工具链支持   | 仅提供最基础的文本生成与对话能力，高级编排需完全自研       |
| **学习与维护成本** | 对 Java 开发者学习曲线平缓，代码规范易于团队长期维护                | 需要团队具备 Python 研发能力，且版本迭代较快，API 变动频繁 | 学习门槛最低，但随着业务复杂度上升，后期维护成本剧增       |




# 对话机器人——快速入门


使用 Spring AI 开发一个大模型对话应用，核心只需要走完标准的三步曲：**引入依赖 $\rightarrow$ 配置模型 $\rightarrow$ 编写客户端调用**。


### 第一步：引入依赖（Maven 配置）

在项目的 `pom.xml` 中，首先需要引入 Spring AI 的版本物料清单（BOM），以便统一管理所有 AI 组件的版本：


```XML
<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.ai</groupId>
            <artifactId>spring-ai-bom</artifactId>
            <version>${spring-ai.version}</version>
            <type>pom</type>
            <scope>import</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

接着，根据你选择的大模型接入方式，引入对应的 **Starter 依赖**：


- 本地模型（如 Ollama）：
    
      
    ```XML
    <dependency>
        <groupId>org.springframework.ai</groupId>
        <artifactId>spring-ai-ollama-spring-boot-starter</artifactId>
    </dependency>
    ```
    
- 云端 API（如兼容 OpenAI 协议的通义千问、DeepSeek 等）：
    
      
    ```XML
    <dependency>
        <groupId>org.springframework.ai</groupId>
        <artifactId>spring-ai-openai-spring-boot-starter</artifactId>
    </dependency>
    ```
    

### 第二步：配置模型参数（`application.yml`）

在 `src/main/resources/application.yml` 中配置大模型的连接地址和具体参数。

  

- **方案 A：连接本地 Ollama 模型**
    
      
    ```YAML
    spring:
      ai:
        ollama:
          base-url: http://localhost:11434 # 本地 Ollama 服务地址
          chat:
            model: deepseek-r1:7b          # 本地运行的模型名称
    ```
    
- **方案 B：连接云端开放 API（以兼容 OpenAI 的平台为例）**
    
      
    ```YAML
    spring:
      ai:
        openai:
          base-url: https://dashscope.aliyuncs.com/compatible-mode # 云端 API 网关
          api-key: ${OPENAI_API_KEY}                               # 你的 API 密钥
          chat:
            options:
              model: qwen-max                                     # 模型名称
              temperature: 0.8                                    # 采样温度（控制回答的随机性）
    ```
    

### 第三步：配置客户端与调用（Java 业务代码）

Spring AI 提供了现代化的 `ChatClient` 接口。你可以通过 `@Bean` 初始化它，然后通过链式调用与大模型交互。

  
#### 1. 注册 `ChatClient` Bean（可预设全局人设）


```Java
@Configuration
public class AiConfig {

    @Bean
    public ChatClient chatClient(ChatModel chatModel) {
        return ChatClient.builder(chatModel)
                .defaultSystem("你是一个可爱的助手，名字叫小团团") // 统一的系统提示词（人设）
                .build();
    }
}
```

#### 2. 在业务层发起对话调用

- **同步阻塞调用（一次性返回完整结果）**：
    
      
    ```Java
    @RestController
    public class ChatController {
    
        @Autowired
        private ChatClient chatClient;
    
        @GetMapping("/chat")
        public String chat() {
            return chatClient.prompt()
                    .user("你是谁？")
                    .call()
                    .content(); // 获取生成的文本内容
        }
    }
    ```
    
- **流式异步调用（实现前端“打字机”效果 / SSE）**：
    
      
    ```Java
    @GetMapping(value = "/stream", produces = "text/html;charset=utf-8")
    //produces参数要特殊设置，否则乱码
    public Flux<String> streamChat() {
        return chatClient.prompt()
                .user("你是谁？")
                .stream()
                .content(); // 返回 Flux<String> 实时推送字符片段
    }
    ```
    

通过这三步，一个基于 Spring Boot 的 AI 对话机器人就已经搭建完成了。无论是连本地的开源模型还是云端的商业模型，Java 代码层面的写法完全保持一致！



# 对话机器人——会话日志

在 Spring AI 的开发中，为了对大模型交互过程进行统一管理、监控和扩展，框架引入了一个非常强大的核心组件——**Advisor（拦截增强机制）**。

![[Pasted image 20261012193706.png]]



### 一、 核心概念：什么是 Advisor？

- **基本定义**：Spring AI 利用了类似于 Spring 传统的 **AOP（面向切面编程）原理**，在 AI 会话发送和响应的过程中提供了拦截与增强功能。
    
      
    
- **核心作用**：它可以在 Prompt 发送给大模型之前（`Before advising`）对其进行修改、日志记录或注入上下文，也可以在大模型返回结果之后（`After advising`）进行过滤、记录或后处理。
    
      
    

### 二、 内置的三大常用 Advisor 类型

在 Spring AI 的设计中，`Advisor` 接口衍生出了多种开箱即用的实现类，用于满足不同的业务需求：

  

- **`SimpleLoggerAdvisor`**：用于打印 AI 会话的详细日志，能够清晰记录发送给大模型的 Prompt 以及返回的响应内容。
    
      
    
- **`MessageChatMemoryAdvisor`**：用于处理多轮对话的上下文记忆（Chat Memory），自动将历史聊天记录维护到对话上下文中。
    
      
    
- **`QuestionAnswerAdvisor`**：用于实现 **RAG（检索增强生成）**，在用户提问时自动去向量知识库中检索相关片段并附加到 Prompt 中。
    
      
    

### 三、 工作原理与执行流程

当客户端发起对话请求时，会经历以下标准的拦截与增强生命周期：

  

1. **Prompt 转换**：将原始的 Prompt 包装为 `AdvisedRequest`。
    
      
    
2. **前置增强（Before advising）**：在调用模型前执行切面逻辑（如日志记录、追加记忆）。
    
      
    
3. **模型调用**：将处理后的请求发送给底层的大模型（`Chat Model`）。
    
      
    
4. **后置增强（After advising）**：大模型返回 `ChatResponse` 后，通过切面进行响应拦截与处理（如记录返回日志）。
    
      
    
5. **最终输出**：将增强后的响应转换为标准的 `ChatResponse` 返回给业务层。
    
      
    

### 四、 代码配置与使用方式

在 Spring Boot 中配置日志拦截器非常简单，只需在构建 `ChatClient` 时通过 `.defaultAdvisors()` 注入 `SimpleLoggerAdvisor` 即可：


```Java
@Bean
public ChatClient chatClient(OllamaChatModel model) {
    return ChatClient.builder(model) // 创建 ChatClient 工厂实例
            .defaultSystem("你是一个可爱的小助手，名字叫小团团。") // 设置默认系统人设
            .defaultAdvisors(new SimpleLoggerAdvisor()) // 配置日志 Advisor，实现全链路会话拦截
            .build(); // 构建 ChatClient 实例
}
```

```yml
spring:
  application:
    name: heima-ai
  ai:
    ollama:
      base-url: http://localhost:11434
      chat:
        model: deepseek-r1:7b

logging:
  level:
    org.springframework.ai.chat.client.advisor: debug
    com.itheima.ai: debug

```

#### 1. `org.springframework.ai.chat.client.advisor: debug`

- **作用对象**：Spring AI 框架层的 Advisor 组件（例如我们配置的 `SimpleLoggerAdvisor`）。
    
- **为什么开启**：Spring AI 的很多拦截和调试日志（比如发送给大模型的最终 Prompt、大模型返回的原始响应、耗时等）默认是处于 `DEBUG` 级别的。如果不把这个包的日志级别调到 `debug`，控制台就不会打印出 AI 交互的详细过程。
    
    
- 让你能**看清 AI 说了什么、传了什么**（框架层）。
    


#### 2. `com.itheima.ai: debug`

- **作用对象**：你自己的项目代码包（即你在项目中编写的 Controller、Service 等所有属于 `com.itheima.ai` 命名空间的代码）。
    
- **为什么开启**：方便你在写自己的 Java 业务逻辑时，随时使用 `log.debug(...)` 打印一些中间变量、参数校验结果或排查业务 Bug。
	
- 让你能**看清你自己的代码运行得怎么样**（业务层）。
	

配置好 `SimpleLoggerAdvisor` 后，每次你与 AI 进行对话，控制台都会自动打印出完整的请求参数、Prompt 拼接结果以及大模型的响应耗时与内容，极大地提升了开发和调试的效率！


# 对话机器人——会话记忆


### 一、 核心原理：大模型为什么需要会话记忆？

大模型本身是“无状态（Stateless）”的，它不记得你上一次跟它聊了什么。如果想让 AI 拥有上下文记忆，唯一的办法就是：**在每次向大模型发送新的提示词时，把之前的历史对话记录（包括用户说的和 AI 回答的）一并打包发给大模型**。


在多轮对话中，消息通常分为三个角色：


- **`system`**：系统提示词（设定角色和全局背景）。
    
- **`user`**：用户输入的指令或问题。
    
- **`assistant`**：大模型在上一次对话中生成的回复。
    


### 二、 Spring AI 实现会话记忆的三步曲

Spring AI 通过 `ChatMemory` 接口和 `MessageChatMemoryAdvisor` 拦截器优雅地实现了多轮对话记忆。

  
#### 第一步：定义会话存储方式（`ChatMemory`）

你需要告诉 Spring AI 将历史聊天记录存在哪里。通常在单机或入门测试时，使用内存存储 `InMemoryChatMemory`：


```Java
@Configuration
public class AiConfig {
	@Bean
	public ChatMemory chatMemory() {
	    return new InMemoryChatMemory(); // 基于内存的会话记忆存储
	}
}
```

#### 第二步：配置会话记忆 Advisor

在创建 `ChatClient` 时，通过 `.defaultAdvisors()` 注入 `MessageChatMemoryAdvisor`，并把上面定义好的 `chatMemory` 传进去：


```Java
@Configuration
public class AiConfig {
	@Bean
	public ChatMemory chatMemory() {
	    return new InMemoryChatMemory(); // 基于内存的会话记忆存储
	}
	
	@Bean
	public ChatClient chatClient(OllamaChatModel model, ChatMemory chatMemory) {
	    return ChatClient.builder(model)
            .defaultSystem("你是一个热心、可爱的智能助手，名字叫小团团，请以小团团的身份与语气回答问题。")
            .defaultAdvisors(
                new SimpleLoggerAdvisor(), // 日志拦截器
                new MessageChatMemoryAdvisor(chatMemory) // 会话记忆拦截器
            )
            .build();
}
}
```

#### 第三步：在请求中添加会话 ID（`chatId`）

当不同的用户或者不同的对话窗口和 AI 交流时，需要通过 `.advisors(...)` 动态指定一个唯一的 `chatId`，以便让系统知道该去内存中提取哪段历史记录：


```Java
@RestController
@RequestMapping("/ai")
@RequiredArgsConstructor
public class ChatController {

    private final ChatClient chatClient;

    @GetMapping(value = "/chat", produces = "text/html;charset=utf-8")
    public Flux<String> chat(String prompt, String chatId) {
        return chatClient.prompt()
                .user(prompt)
                .advisors(a -> a.param(AbstractChatMemoryAdvisor.CHAT_MEMORY_CONVERSATION_ID_KEY, chatId)) // 绑定当前会话 ID
                .stream()
                .content(); // 流式返回打字机效果
    }
}
```

### 三、 总结

通过引入 `MessageChatMemoryAdvisor` 和 `chatId` 参数：


1. **自动存取**：框架会自动在每次请求前把历史消息拼接到 Prompt 中，并在对话结束后把最新的问答追加到内存里。
    
2. **多用户隔离**：通过传入不同的 `chatId`（例如用户 A 的 ID 和用户 B 的 ID），系统能够完美隔离多个用户的对话上下文，互不干扰！



# 对话机器人——会话历史功能

为了支持前端的左侧列表渲染和点击后的详情回显，我们需要在后端编写对应的 Controller 和管理逻辑。

![[Pasted image 20261013192640.png]]

由于默认的 `InMemoryChatMemory` 主要是按 `chatId`（会话 ID）存储消息的，并没有直接提供“按业务类型分类查询所有会话 ID”的方法。因此，在实际开发中，我们通常会引入一个轻量级的会话元数据管理（或使用一个线程安全的 Map/服务来记录 `type` 与 `chatId` 的关联）。

  
### 1. 配置类：定义 `ChatMemory` 并组装 `ChatClient`

在这一步中，我们注册了基于内存的 `ChatMemory`，并在 `ChatClient` 中挂载了 `SimpleLoggerAdvisor` 和 `MessageChatMemoryAdvisor`。


```Java
package com.itheima.ai.config;

import org.springframework.ai.chat.client.ChatClient;
import org.springframework.ai.chat.client.advisor.MessageChatMemoryAdvisor;
import org.springframework.ai.chat.client.advisor.SimpleLoggerAdvisor;
import org.springframework.ai.chat.memory.ChatMemory;
import org.springframework.ai.chat.memory.InMemoryChatMemory;
import org.springframework.ai.ollama.OllamaChatModel;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class AiConfig {

    /**
     * 第一步：定义会话存储方式（基于内存）
     */
    @Bean
    public ChatMemory chatMemory() {
        return new InMemoryChatMemory();
    }

    /**
     * 第二步：配置会话记忆 Advisor 并构建 ChatClient
     */
    @Bean
    public ChatClient chatClient(OllamaChatModel model, ChatMemory chatMemory) {
        return ChatClient.builder(model)
                .defaultSystem("你是一个热心、可爱的智能助手，名字叫小团团，请以小团团的身份与语气回答问题。")
                .defaultAdvisors(
                        new SimpleLoggerAdvisor(),                      // 日志拦截器
                        new MessageChatMemoryAdvisor(chatMemory)        // 会话记忆拦截器
                )
                .build();
    }
}
```

### 2. 服务层：管理会话历史与分类

默认的 `InMemoryChatMemory` 只按 `chatId` 存储，我们需要一个辅助服务来支持前端按业务类型（`type`）查询会话列表。



```Java
package com.itheima.ai.service;

import org.springframework.ai.chat.memory.ChatMemory;
import org.springframework.ai.chat.messages.Message;
import org.springframework.stereotype.Service;

import java.util.ArrayList;
import java.util.List;
import java.util.Map;
import java.util.concurrent.ConcurrentHashMap;

@Service
public class ChatHistoryService {

    // 模拟存储：业务类型 -> 会话ID列表
    private final Map<String, List<String>> typeToChatIdsMap = new ConcurrentHashMap<>();
    
    private final ChatMemory chatMemory;

    public ChatHistoryService(ChatMemory chatMemory) {
        this.chatMemory = chatMemory;
    }

    /**
     * 记录并归类会话 ID
     */
    public void addSession(String type, String chatId) {
        typeToChatIdsMap.computeIfAbsent(type, k -> new ArrayList<>());
        List<String> chatIds = typeToChatIdsMap.get(type);
        if (!chatIds.contains(chatId)) {
            chatIds.add(0, chatId); // 新会话加在最前面
        }
    }

    /**
     * 根据类型获取会话列表
     */
    public List<String> getChatIdsByType(String type) {
        return typeToChatIdsMap.getOrDefault(type, new ArrayList<>());
    }

    /**
     * 根据 chatId 获取该会话的所有历史消息详情
     */
    public List<Message> getChatMessages(String chatId) {
        return chatMemory.get(chatId, 100); // 获取最近 100 条消息
    }
}
```

### 3. 控制层：绑定会话 ID 并提供历史记录查询

在这里，我们在对话请求中通过 `.advisors(...)` 动态绑定了当前请求的 `chatId`，并补全了前端需要的历史会话列表和详情接口。


```Java
package com.itheima.ai.controller;

import com.itheima.ai.service.ChatHistoryService;
import lombok.RequiredArgsConstructor;
import org.springframework.ai.chat.client.ChatClient;
import org.springframework.ai.chat.client.advisor.AbstractChatMemoryAdvisor;
import org.springframework.ai.chat.messages.Message;
import org.springframework.web.bind.annotation.*;
import reactor.core.publisher.Flux;

import java.util.List;

@RestController
@RequestMapping("/ai")
@RequiredArgsConstructor
public class ChatController {

    private final ChatClient chatClient;
    private final ChatHistoryService chatHistoryService;

    /**
     * 第三步：流式对话接口（绑定 chatId 和 type）
     */
    @GetMapping(value = "/chat", produces = "text/html;charset=utf-8")
    public Flux<String> chat(String prompt, String chatId, @RequestParam(defaultValue = "default") String type) {
        // 1. 将会话记录到分类服务中
        chatHistoryService.addSession(type, chatId);

        // 2. 调用大模型，并通过 advisor 动态绑定 chatId 上下文记忆
        return chatClient.prompt()
                .user(prompt)
                .advisors(a -> a.param(AbstractChatMemoryAdvisor.CHAT_MEMORY_CONVERSATION_ID_KEY, chatId))
                .stream()
                .content();
    }

    /**
     * 查询会话记录列表接口
     */
    @GetMapping("/history/{type}")
    public List<String> getChatList(@PathVariable String type) {
        return chatHistoryService.getChatIdsByType(type);
    }

    /**
     * 查询会话记录详情接口
     */
    @GetMapping("/history/{type}/{chatId}")
    public List<Message> getChatDetail(@PathVariable String type, @PathVariable String chatId) {
        return chatHistoryService.getChatMessages(chatId);
    }
}
```


### 代码逻辑说明

1. **对话绑定**：当用户在前端发起 `/ai/chat?prompt=xxx&chatId=1241&type=work` 对话时，后端会自动调用 `chatHistoryService.addSession("work", "1241")`，把会话归类到对应的业务类型中。
    
      
    
2. **列表查询**：前端请求 `/ai/history/work` 时，会返回该类型下的所有会话 ID 数组（例如 `["1241", "1246"]`）。
    
      
    
3. **详情回显**：当用户点击左侧某一个会话时，前端请求 `/ai/history/work/1241`，后端直接从内存中的 `ChatMemory` 读取出该 `chatId` 对应的完整消息对象列表，返回给前端进行聊天气泡的渲染。



# 提示词工程


提示词工程（Prompt Engineering）是与大语言模型（LLM）进行高效沟通的核心艺术与科学。

如果把大模型比作一个拥有海量知识但缺乏具体上下文的“超级天才”，那么**提示词就是给这位天才下达的任务说明书**。提示词写得好坏，直接决定了模型输出的结果是“惊艳”还是“胡言乱语”。

我们可以从**核心结构、经典框架、高阶技巧以及编写原则**四个维度，来系统性地认识提示词工程。

## 概念介绍


### 一、 优秀提示词的四大核心要素

一个结构清晰、高效的提示词，通常不会是一句简单的“帮我写个XX”，而是包含以下几个关键部分：

  

1. **角色（Role）**
    
      
    - **作用**：为大模型设定一个特定的身份或专业背景，唤醒其在海量数据中对应领域的专业知识。
        
          
        
    - **示例**：“你是一位拥有 10 年架构经验的高级 Java 专家。”
        
          
        
2. **背景与上下文（Context）**
    
      
    - **作用**：提供当前任务发生的真实场景、前提条件或业务痛点。
        
          
        
    - **示例**：“目前公司正在将传统单体系统改造成微服务架构，团队成员对 Spring Cloud 的掌握程度参差不齐。”
        
          
        
3. **明确任务（Task / Instruction）**
    
      
    - **作用**：清晰、无歧义地告诉模型需要完成的具体工作。
        
          
        
    - **示例**：“请针对团队现状，制定一份为期四周的 Spring Cloud 渐进式改造培训大纲。”
        
          
        
4. **约束条件与输出格式（Constraints & Format）**
    
      
    - **作用**：划定边界（防止模型偷懒、幻觉或长篇大论），并规定最终返回的呈现形式。
        
          
        
    - **示例**：“要求：1. 每天的培训内容包含核心理论与实战演练；2. 严禁出现空洞的理论；3. 必须以 Markdown 表格形式输出。”
        
          
        

### 二、 经典的提示词编写框架：COSTAR 框架

在工程实践中，为了避免遗漏关键信息，国际上广泛流传着一些高效的提示词设计框架，其中 **COSTAR 框架**最受推崇：

  

- **C - Context（背景）**：任务发生的背景情况。
    
      
    
- **O - Objective（目标）**：你希望模型为你实现什么具体目标。
    
      
    
- **S - Style（风格）**：希望模型以什么文风来写（如：学术论文、科技博客、幽默风趣）。
    
      
    
- **T - Tone（语气）**：回答的语气基调（如：专业、严厉、鼓励、客观）。
    
      
    
- **A - Audience（受众）**：这段内容是写给谁看的（如：初级程序员、非技术高管、大众用户）。
    
      
    
- **R - Response（输出格式）**：最终呈现的结构（如：JSON、分点列表、表格）。
    
      
    

### 三、 提示词工程的高阶技巧

当面对复杂的业务逻辑时，单纯靠堆砌描述往往不够，需要运用一些高阶提示词策略：

  

1. **思维链（Chain of Thought, CoT）**
    
      
    - **核心思想**：引导模型“一步一步地思考”。
        
          
        
    - **经典句式**：“请先分析问题的核心难点，然后一步步推导，最后得出结论。” 这能显著提升大模型在数学计算、代码逻辑和复杂推理上的准确率。
        
          
        
2. **少样本提示（Few-Shot Prompting）**
    
      
    - **核心思想**：不要只告诉模型规则，还要给它看“样板间”。
        
          
        
    - **做法**：在提示词中提供 2 到 3 个“输入-输出”的完美范例。模型会模仿范例的格式和逻辑，产出高度一致的结果。
        
          
        
3. **负向提示（Negative Prompting）**
    
      
    - **核心思想**：明确告诉模型**不要做什么**。
        
          
        
    - **示例**：“在回答时，不要使用任何行业黑话，不要出现超过 3 行的段落。”（有时候，限制边界比开放发挥更重要）。
        
          
        
4. **角色反思与自我检查（Self-Correction）**
    
      
    - **核心思想**：让模型充当“裁判”检查自己的工作。
        
          
        
    - **示例**：“请先生成一份方案，然后切换到资深技术总监的视角，对该方案提出 3 个潜在的架构漏洞并进行修正。”
        
          
        

### 四、 开发中的黄金准则

1. **具体胜过抽象**：与其说“写一段高质量的代码”，不如说“写一段符合阿里巴巴 Java 开发规范、带有详细异常处理的线程池工具类”。
    
      
    
2. **迭代思维**：提示词很少有一版成型的。把它当作代码来写——编写 $\rightarrow$ 测试 $\rightarrow$ 发现输出不符合预期 $\rightarrow$ 调整优化。
    
      
    
3. **善用分隔符**：当提示词中包含大段外部文本（如用户输入的文章、日志）时，使用 `"""`、`###` 或 XML 标签（如 `<text>...</text>`）将指令与数据隔离开，防止模型被恶意输入“越狱”或篡改指令。
    
      
    

把前面讲的**提示词工程理论**落地到 **Spring AI** 中，你会发现 Spring AI 并不是简单地让你去拼接字符串，而是通过**强类型、面向对象和声明式 API** 的方式，把提示词工程变成了规范的 Java 代码。



## Spring AI 中的提示词工程


在 Spring AI 中，提示词工程主要通过以下四大核心机制来实现：


### 一、 动态提示词模板：`PromptTemplate`

在实际业务中，提示词往往需要动态填充用户输入的参数（比如商品名称、用户年龄、业务数据）。Spring AI 提供了 `PromptTemplate`，完美支持类似 Thymeleaf 或标准占位符的语法。

  

- **理论对应**：任务（Task）与背景（Context）的动态组合。
    
      
    
- **代码实现**：
    
      
    ```Java
    // 1. 定义带有占位符的模板
    PromptTemplate promptTemplate = new PromptTemplate(
        "你是一位资深的{industry}专家。请针对用户提出的问题：'{question}'，提供一份包含3个核心要点的专业解答。"
    );
    
    // 2. 传入动态参数并创建 Prompt
    Prompt prompt = promptTemplate.create(Map.of(
        "industry", "金融科技",
        "question", "中小企业如何进行数字化转型？"
    ));
    
    // 3. 发送给大模型
    String response = chatClient.prompt(prompt).call().content();
    ```
    

### 二、 角色分层管理：`Message` 体系

前面提到提示词要分角色（System、User、Assistant）。Spring AI 将这些角色抽象为了强类型的消息对象：

  

- `SystemMessage`：对应全局人设、约束条件（通常在构建 `ChatClient` 时通过 `.defaultSystem()` 或在代码中显式指定）。
    
      
    
- `UserMessage`：对应用户的即时输入。
    
      
    
- `AssistantMessage`：对应大模型的历史回复（在实现多轮对话时由框架自动维护）。
    
      
    

通过 `Prompt` 对象可以把多条消息打包发送：


```Java
Prompt prompt = new Prompt(List.of(
    new SystemMessage("你是一个严谨的代码审阅机器人，只指出Bug，不夸奖。"),
    new UserMessage("public void test() { int a = 1 / 0; }")
));
```

### 三、 声明式链式编排：`ChatClient` 的提示词组合

在 Spring AI 的现代化 `ChatClient` API 中，提示词工程的编写被设计成了优雅的链式调用，你可以直接在代码中组合系统提示词和用户提示词：


```Java
String result = chatClient.prompt()
        .system(s -> s.text("你是一个精通Spring Boot的架构师，请用幽默的语气回答。")) // 设定系统提示词
        .user(u -> u.text("什么是循环依赖？它是怎么产生的？"))                    // 设定用户提示词与任务
        .call()
        .content();
```

### 四、 进阶：用提示词约束实现“结构化输出”（Structured Output）

在企业级开发中，最头疼的是大模型每次返回的文本格式都不一样。提示词工程中强调的“严格规定输出格式”，在 Spring AI 中可以直接映射为 **Java 实体类**。

  
你可以通过提示词引导模型，配合 Spring AI 的 `.entity()` 方法，让大模型直接把结果转成强类型的 Java Bean：


```Java
// 定义你希望大模型返回的结构
record ProductAnalysis(String productName, double suggestedPrice, List<String> sellingPoints) {}

// 在代码中通过提示词+实体类映射
ProductAnalysis analysis = chatClient.prompt()
        .user("分析一下这款新型智能保温杯的市场竞争力")
        .call()
        .entity(ProductAnalysis.class); // Spring AI 会在底层自动通过提示词约束模型输出 JSON，并反序列化为该对象
```

### 总结

在 Spring AI 中：
  

1. **提示词不是散乱的文本**，而是通过 `PromptTemplate` 进行规范化管理的模板。
    
      
    
2. **约束不再只是口头要求**，而是可以结合 Spring AI 的 `entity()` 转换成强类型的 Java 对象。
    
      
    
3. **角色不再需要手动拼装**，由 `ChatClient` 和 Advisor 自动在底层组织成标准的 OpenAI 兼容消息数组。
    
      
    


# 哄哄模拟器——代码实现


对于哄哄模拟器，我们**精简掉了会话历史功能**（移除了历史列表与详情查询接口、以及对应的辅助 Service），一轮游戏玩完就结束，只保留最核心、最干净的 **Spring AI 核心对话能力 + ChatMemory 多轮记忆 + 日志拦截器**


### 一、 配置文件 (`application.yaml`)

配置云端大模型（以兼容 OpenAI 协议的平台为例）以及调试日志级别：


```YAML
spring:
  application:
    name: heima-ai
  ai:
    openai:
      base-url: https://dashscope.aliyuncs.com/compatible-mode
      api-key: ${OPENAI_API_KEY}
      chat:
        options:
          model: qwen-max
          temperature: 0.9  # 调高温度，让模拟器对话更有趣味性和变化

logging:
  level:
    org.springframework.ai.chat.client.advisor: debug
    com.itheima.ai: debug
```

### 二、 核心配置类 (`AiConfig.java`)

在配置类中注册内存记忆组件 `ChatMemory`，并组装带有**日志拦截**和**多轮记忆拦截**的 `ChatClient`，同时预设好“哄哄模拟器”的系统人设：


```Java
package com.itheima.ai.config;

import org.springframework.ai.chat.client.ChatClient;
import org.springframework.ai.chat.client.advisor.MessageChatMemoryAdvisor;
import org.springframework.ai.chat.client.advisor.SimpleLoggerAdvisor;
import org.springframework.ai.chat.memory.ChatMemory;
import org.springframework.ai.chat.memory.InMemoryChatMemory;
import org.springframework.ai.openai.OpenAiChatModel;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class AiConfig {

    /**
     * 1. 定义会话存储方式（单机内存记忆）
     */
    @Bean
    public ChatMemory chatMemory() {
        return new InMemoryChatMemory();
    }

    /**
     * 2. 配置并组装 ChatClient（注入人设与 Advisor 拦截器）
     */
    @Bean
    public ChatClient chatClient(OpenAiChatModel model, ChatMemory chatMemory) {
        return ChatClient.builder(model)
                .defaultSystem(
                    "你现在是一个正在生气的男朋友/女朋友。你的任务是扮演“哄哄模拟器”的主角。\n" +
                    "规则：\n" +
                    "1. 用户需要通过各种言语来哄你开心。\n" +
                    "2. 你需要根据用户说的话来判断诚意，并在回复的最后附带当前的好感度（0-100分）。如果好感度达到100分则表示哄成功了。\n" +
                    "3. 格式要求：回复内容 + 换行标注 [当前好感度: X分]。"
                )//编写一个常量类内置这些提示词成常量也可以
                .defaultAdvisors(
                        new SimpleLoggerAdvisor(),                      // 全链路日志拦截器
                        new MessageChatMemoryAdvisor(chatMemory)        // 多轮对话记忆拦截器
                )
                .build();
    }
}
```

### 三、 业务控制器 (`ChatController.java`)

只保留核心的对话接口。通过传入不同的 `chatId` 来隔离不同用户的对话上下文，实现流式（打字机效果）输出：

```Java
package com.itheima.ai.controller;

import lombok.RequiredArgsConstructor;
import org.springframework.ai.chat.client.ChatClient;
import org.springframework.ai.chat.client.advisor.AbstractChatMemoryAdvisor;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import reactor.core.publisher.Flux;

@RestController
@RequestMapping("/ai")
@RequiredArgsConstructor
public class ChatController {

    private final ChatClient chatClient;

    /**
     * 哄哄模拟器流式对话接口
     * @param prompt 用户的输入内容
     * @param chatId 会话唯一ID（用于区分不同窗口或用户的记忆）
     */
    @GetMapping(value = "/chat", produces = "text/html;charset=utf-8")
    public Flux<String> chat(String prompt, String chatId) {
        return chatClient.prompt()
                .user(prompt)
                // 通过 Advisor 上下文动态绑定当前会话的 chatId
                .advisors(a -> a.param(AbstractChatMemoryAdvisor.CHAT_MEMORY_CONVERSATION_ID_KEY, chatId))
                .stream()
                .content();
    }
}
```




# Function Calling


**Function Calling（函数调用）** 是大语言模型（LLM）领域一项极其核心的技术，它彻底改变了模型与外部世界交互的方式，使大模型从一个“只会凭静态知识瞎聊的文本生成器”真正升级为**具备实际行动能力的智能体（Agent）**。

## 概念介绍

### 一、 什么是 Function Calling？

简单来说，**Function Calling 允许大模型在面对用户的自然语言提问时，识别出需要借助外部工具或 API 才能完成任务，并输出一段结构化的数据（通常是 JSON 格式）来指导应用程序去调用相应的函数**。


大模型本身并不直接执行外部代码或查询实时数据库，它只负责“**动脑思考**”：判断应该调用哪个函数、传入什么参数；而真正的“**动手执行**”则由你的后端应用程序来完成。

  

### 二、 为什么需要 Function Calling？

大模型天生有两大痛点：

  

1. **静态知识盲区**：它们的训练数据有截止日期，无法知道实时信息（如今天的实时天气、股票价格、最新新闻）。
    
      
    
2. **无法隔离与操作业务系统**：模型无法直接连通你的私有数据库、企业内部 ERP 或发送邮件。
    
      
    

通过 Function Calling，大模型充当了**自然语言和后端代码之间的“翻译官”和“总调度”**，让用户可以用大白话直接操作复杂的软件系统。

  

### 三、 Function Calling 的标准工作流程

整个交互过程通常分为以下几个步骤（以“查询北京天气”为例）：

  

1. **注册工具定义（准备阶段）**：
    
    开发者在向大模型发送请求时，会同时把系统里有哪些函数（Tool/Function）告诉大模型。每一个函数都需要提供：
    
      
    - **函数名称**：例如 `getWeather`
        
          
        
    - **功能描述**：告诉模型这个函数是干什么的（描述越精准，模型越会用）
        
          
        
    - **入参规范（JSON Schema）**：规定这个函数需要哪些参数、类型是什么
        
          
        
2. **用户发起提问**：
    
    用户输入：“帮我查一下北京今天的天气。”
    
      
    
3. **模型识别并输出指令（第一次大模型调用）**：
    
    大模型分析用户的意图，发现光靠脑子回答不了，但匹配到了注册的 `getWeather` 函数。此时，**模型不会直接返回文字回答**，而是返回一个结构化指令：
    
      
    - _意图_：需要调用函数 `getWeather`
        
          
        
    - _参数_：`{"city": "北京"}`
        
          
        
4. **应用程序拦截并执行（本地逻辑）**：
    
    你的后端程序捕获到模型的这个指令，在本地真正去请求第三方天气 API（或者查数据库），拿到了返回结果：`{"temperature": "25℃", "weather": "晴"}`。
    
      
    
5. **将结果回传给模型（第二次大模型调用）**：
    
    应用程序把刚才查到的天气数据再次打包发给大模型：“这是刚才调用的执行结果。”
    
      
    
6. **生成最终答复**：
    
    大模型结合这个结果，用通俗易懂的自然语言组织好语言回复给用户：“北京今天天气晴，气温 25 摄氏度。”
    
      
    

### 四, 典型应用场景

- **实时信息检索**：让大模型实时查询股票、汇率、航班、快递物流等。
    
      
    
- **企业系统集成**：例如用户说“帮我把张三的账户余额扣减 100 元”，模型通过 Function Calling 锁定对应的扣款 API 并提取参数。
    
      
    
- **数据结构化提取**：将一段杂乱无章的用户对话（如简历文本）通过定义好的函数结构直接清洗并转换为标准的数据对象。



## Spring AI 中的 Function Calling

将前面介绍的 **Function Calling（函数调用）** 核心思想落地到 **Spring AI** 中，你会发现 Spring AI 做了一件非常优雅的事情：**它把复杂的底层 JSON Schema 封装了起来，让你可以直接用标准的 Java 函数（Lambda 或 Bean）来充当大模型的外部工具。**


在 Spring AI 中，实现 Function Calling 通常只需要两步：**定义 Java 函数（Tool）** $\rightarrow$ **在 `ChatClient` 中注册并调用**。

  
### 一、 Spring AI 实现 Function Calling 的核心步骤

#### 1. 定义输入输出实体与函数 Bean

Spring AI 依托于 Java 的 `java.util.function.Function` 接口。你可以将任何一个普通的业务方法注册为 Spring Bean。为了让大模型知道这个函数是干什么的、需要什么参数，通常会结合实体类（如 `record`）来定义。


例如，我们实现一个“查询实时天气”的工具：


```Java
package com.itheima.ai.function;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import java.util.function.Function;

@Configuration
public class WeatherConfiguration {

    // 1. 定义入参实体类
    public record WeatherRequest(String city, String date) {}

    // 2. 定义出参实体类
    public record WeatherResponse(String city, String temperature, String weather) {}

    // 3. 注册为 Spring Bean，名字叫 getWeather
    @Bean
    public Function<WeatherRequest, WeatherResponse> getWeather() {
        return request -> {
            // 这里可以编写真实的第三方天气 API 调用逻辑
            System.out.println(">> 正在调用本地天气服务，查询城市: " + request.city());
            
            // 模拟返回结果
            if ("北京".equals(request.city())) {
                return new WeatherResponse("北京", "26℃", "晴");
            } else {
                return new WeatherResponse(request.city(), "22℃", "多云");
            }
        };
    }
}
```

#### 2. 在 `ChatClient` 中挂载并调用函数

当用户提问时，你只需要在代码中通过 `.functions(...)` 把刚才注册的函数名称传进去。大模型会自动判断是否需要调用它：


```Java
package com.itheima.ai.controller;

import lombok.RequiredArgsConstructor;
import org.springframework.ai.chat.client.ChatClient;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
@RequestMapping("/ai")
@RequiredArgsConstructor
public class ChatController {

    private final ChatClient chatClient;

    @GetMapping("/chat")
    public String chat(String prompt) {
        return chatClient.prompt()
                .user(prompt)
                .functions("getWeather") // 关键：在这里注册我们刚才定义的 Bean 名称，让大模型拥有这个工具
                .call()
                .content();
    }
}
```

### 二、 整个流程在 Spring AI 底层是如何自动运转的？

当你在浏览器访问 `/ai/chat?prompt=帮我查一下北京今天的天气` 时，Spring AI 在底层会自动完成以下复杂的“黑盒”交互：

  

1. **意图识别与参数提取**：Spring AI 在向大模型发送请求时，会自动把 `getWeather` 函数的结构描述（通过反射和 JSON Schema）一并打包发给大模型。大模型发现用户在问北京天气，便会中断普通的文本回复，转而返回一个结构化指令：“我要调用 `getWeather`，参数是 `{"city": "北京", "date": "今天"}`”。
    
      
    
2. **本地拦截执行**：Spring AI 框架捕获到这个指令，自动在 Spring 容器中找到名为 `getWeather` 的 Bean，并把参数传进去执行，拿到返回的 `WeatherResponse` 对象（`北京, 26℃, 晴`）。
    
      
    
3. **二次封装回传**：框架自动把这个执行结果再次打包，发送给大模型：“这是工具执行拿到的数据。”
    
      
    
4. **生成最终答复**：大模型结合这个结果，输出自然流畅的最终回答：“根据实时查询，北京今天天气晴，气温是 26 摄氏度。”
    
      
    

### 三、 Spring AI 方案的核心优势

1. **类型安全**：参数的接收和返回都是强类型的 Java 对象（POJO / `record`），避免了手写和解析繁琐的 JSON 字符串。
    
      
    
2. **无缝对接 Spring 生态**：工具函数本身就是一个标准的 Spring Bean，这意味着你可以在函数内部直接注入 `Service`、`Mapper` 或 `RestTemplate`，轻松去查你的 MySQL 数据库或调用其他微服务！



# 智能客服——需求和思路分析


  ![[Pasted image 20261014161622.png]]

将 **Function Calling（函数调用）** 技术放到一个具体的企业级场景中去理解，最经典的莫过于打造一个具备业务执行能力的 **AI 智能客服**。


### 一、 业务场景与核心需求

假设我们需要为“黑马程序员”实现一个 24 小时在线的 AI 智能客服“小黑”，其核心目标是：**为学员咨询培训课程，并帮用户预约线下课程试听**。

  
为了达成这个目标，客服不能只会机械地闲聊，它需要引导用户完成一个标准的**转化漏斗流程**：

  

1. **了解、分析用户兴趣与学历信息**。
    
2. **查询满足用户条件的课程**。
    
3. **给用户推荐课程**（业务规则：不直接透露价格，而是通过引导试听来转化）。
    
4. **引导用户预约试听**。
    
5. **查询校区信息**供用户选择。
    
6. **引导学生留下联系方式与姓名**。
    
7. **新增预约单**。
    

### 二、 核心挑战：大模型为什么必须依赖 Function Calling？


大模型虽然聪明，但它本身是一个“孤岛”，无法直接读取公司的课程数据库、校区数据库，也无法往订单表里写入数据。


因此，必须通过 **Prompt（提示词工程）** 来规范客服的行为边界（例如：_必须先获取学历和兴趣才能推荐；预约前必须问清校区和联系方式；严禁直接告知价格_），并通过 **Function Calling** 赋予它操作后端系统的“双手”。

  

### 三、 智能客服中的 Function Calling 工具定义

为了让大模型在对话中能够自主去查数据、做下单动作，我们在后端为它注册了三个核心的 **Tool（函数工具）**：


1. **`queryLesson(xx)`**：
    
      
    - **作用**：根据用户的兴趣、学历等条件，查询满足条件的课程列表。
        
          
        
2. **`querySection(xx)`**：
    
      
    - **作用**：查询全国各校区的信息，供用户选择预约地址。
        
          
        
3. **`saveOrder(xx)`**：
    
      
    - **作用**：收集齐学员的姓名、电话、意向课程和校区后，新增预约单。
        
          
        


### 四、Spring AI 对 Function Calling 的简化


在没有框架封装的传统开发中（或直接调用大模型原生 API），实现这个闭环非常痛苦：

![[Pasted image 20261014162043.png]]

1. 你需要手写繁琐的 **JSON Schema** 来向大模型描述你的函数名、说明和入参格式。
    
      
    
2. 当大模型返回调用指令时，你需要自己解析 JSON，写一堆 `if-else` 或反射代码去**手动分发**到具体的函数执行。
    
      
    
3. 执行完后，你还要手动把结果拼成大模型指定的对话历史格式，**发起第二次请求**传回给模型。
    
      
    

而 **Spring AI** 把这些“脏活累活”全在底层封装好了（Spring AI 框架在中间承担了 `Dispatch Tool Call Requests` 的核心调度职责）：

  ![[Pasted image 20261014162309.png]]

- **自动化（自动生成 Schema）**：你只需要写一个标准的 Java 函数或 `Function` Bean，Spring AI 会利用反射和类型推断，自动把它转成大模型能理解的工具定义。
    
      
    
- **自动化（自动分发调度）**：当大模型在对话中决定调用某个函数时，Spring AI 会自动拦截该指令，在 Spring 容器中找到对应的 Bean，自动把强类型参数传进去执行。
    
      
    
- **自动化（自动闭环回传）**：Spring AI 会自动把函数的执行结果打包，默默地发起二次请求回传给大模型，最终把大模型整理好的自然语言结果交还给你的业务代码。
    
      
    

对于 Java 开发者而言，你只需要专注于编写**业务函数本身**，并在 `ChatClient` 中挂载一下函数名称，复杂的交互闭环和多轮调度全由 Spring AI 在底层自动搞定！

### 五、 结合 Function Calling 的完整交互闭环（Agent 运行流程）

当用户和这个智能客服交互时，底层会触发一个由 Function Calling 驱动的**自动化调度循环**：

  

1. **用户提问**：“我想学Java，不知道适合不适合，帮我看看。”
    
      
    
2. **大模型任务拆分**：
    
      
    - AI 应用将用户的输入、系统提示词以及上述三个工具的定义（Tool Definition，包含函数名、描述和参数规范）一同发送给大模型。
        
          
        
    - 大模型分析发现，用户虽然说了想学Java，但还缺少学历等背景信息，或者需要先调用 `queryLesson` 去检索匹配的 Java 课程。
        
          
        
3. **触发函数调用**：
    
      
    - 大模型中断文本回复，向应用层返回一个结构化指令：“需要调用 `queryLesson` 函数，传入参数：技术方向=Java”。
        
          
        
4. **本地执行与回传**：
    
      
    - 应用程序（在 Spring AI 等框架的调度下）拦截到该指令，在本地安全地执行 `queryLesson`，从数据库中查出具体的课程信息。
        
          
        
    - 应用程序将查询到的课程数据再次打包，作为结果回传给大模型。
        
          
        
5. **多轮递进与最终响应**：
    
      
    - 大模型拿到真实课程数据后，继续引导用户：“我们有适合不同基础的Java课程……请问您目前的学历是怎样的呢？”
        
          
        
    - 后续当用户选定校区并留下电话时，大模型会再次触发 `saveOrder` 函数完成预约单落库，最后输出：“预约成功！”
        
          
        

通过这种方式，**Function Calling 将静态的语言模型与动态的业务系统（课程查询、校区查询、订单系统）无缝缝合在了一起**，让 AI 真正具备了解决实际业务问题的智能客服能力。



# 智能客服——代码实现



整个实现分为三步：**编写 System 提示词** $\rightarrow$ **定义 Tool 类与参数** $\rightarrow$ **在 ChatClient 中配置挂载 Tool**。


### 第一步：编写 System 提示词与常量定义

首先，将客服“小黑”的人设、业务规则和话术约束定义为一个常量字符串：

  
```Java
package com.itheima.ai.constant;

public class PromptConstants {

    public static final String CUSTOMER_SERVICE_SYSTEM = 
            "你是一家名为“黑马程序员”的职业教育公司的智能客服，你的名字叫小黑。\n" +
            "你以热情的方式回应用户，给用户提供课程咨询、预约试听服务。\n\n" +
            "1.课程咨询：\n" +
            "- 提供课程建议前必须从用户那里获得以下信息：学习兴趣(编程、设计、自媒体、其它)、学员学历。\n" +
            "- 然后分析用户信息，梳理用户需求，调用工具查询符合用户需求的空间课程信息，推荐给用户。\n" +
            "- 推荐课程信息时尽量不要透露课程价格，而是想办法让用户预约课程。\n" +
            "- 与用户确认想要了解的课程后，再进入课程预约环节。\n\n" +
            "2.课程预约：\n" +
            "- 在帮助用户预约课程之前，你需要询问学生要去哪个校区试听。\n" +
            "- 可以通过工具查询校区列表，供用户选择预约的校区。\n" +
            "- 你必须从用户那里获得用户的联系方式、姓名、预约的校区信息，才能进行课程预约。\n" +
            "- 收集到预约信息后要跟用户最终确认信息是否正确。\n" +
            "- 信息无误后，调用工具生成课程预约单。\n\n" +
            "最后，告知用户预约成功，并给出简略的预约信息。";
}
```

### 第二步：定义 Tool 类与查询参数对象

使用 Spring AI 提供的 `@Tool` 和 `@ToolParam` 注解，让大模型能够识别并调用该方法（以课程查询为例）：

  
```Java
package com.itheima.ai.tool;

import lombok.Data;
import org.springframework.ai.tool.annotation.Tool;
import org.springframework.ai.tool.annotation.ToolParam;

import java.util.List;

public class CourseTools {

    /**
     * 定义课程查询工具
     */
    @Tool(description = "根据条件查询课程")
    public List<Course> queryCourse(
            @ToolParam(description = "课程查询条件") CourseQuery query) {
        
        // 此处可接入真实的数据库或业务 Service 查询
        System.out.println(">> [Tool执行] 正在根据条件查询课程，类型: " + query.getType() + ", 学历要求: " + query.getEdu());
        
        // 模拟返回查询到的课程数据
        return List.of(
                new Course("Java就业班", "面向零基础，系统掌握企业级Java开发"),
                new Course("AI大模型实战班", "掌握Spring AI与主流大模型应用开发")
        );
    }

    // ================= 内部实体类定义 =================

    @Data
    public static class CourseQuery {
        @ToolParam(required = false, description = "课程类型")
        private String type;

        @ToolParam(required = false, description = "学习背景/学历")
        private Integer edu;

        @ToolParam(required = false, description = "排序方式")
        private List<String> sorts;
    }

    public record Course(String name, String description) {}
}
```

### 第三步：配置 Tool 并组装 ChatClient

在配置类中，通过 `.defaultTools(...)` 将我们写好的 `CourseTools` 挂载到 `ChatClient` 中：

```Java
package com.itheima.ai.config;

import com.itheima.ai.tool.CourseTools;
import org.springframework.ai.chat.client.ChatClient;
import org.springframework.ai.chat.client.advisor.MessageChatMemoryAdvisor;
import org.springframework.ai.chat.client.advisor.SimpleLoggerAdvisor;
import org.springframework.ai.chat.memory.ChatMemory;
import org.springframework.ai.chat.memory.InMemoryChatMemory;
import org.springframework.ai.openai.OpenAiChatModel;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

import static com.itheima.ai.constant.PromptConstants.CUSTOMER_SERVICE_SYSTEM;

@Configuration
public class AiConfig {

    @Bean
    public ChatMemory chatMemory() {
        return new InMemoryChatMemory();
    }

    @Bean
    public ChatClient serviceChatClient(
            OpenAiChatModel model,
            ChatMemory chatMemory) {
        
        return ChatClient.builder(model)
                .defaultSystem(CUSTOMER_SERVICE_SYSTEM) // 1. 注入客服系统提示词
                .defaultAdvisors(
                        new MessageChatMemoryAdvisor(chatMemory), // 2. 多轮会话记忆
                        new SimpleLoggerAdvisor()                // 3. 日志拦截
                )
                .defaultTools(new CourseTools())                // 4. 关键：配置并挂载 Function Calling 工具
                .build();
    }
}
```



### 第四步：控制器类


```java
package com.itheima.ai.controller;

import lombok.RequiredArgsConstructor;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;
import reactor.core.publisher.Flux;
import com.itheima.ai.constant.SystemConstants;
import com.itheima.ai.client.ChatClient;
import com.itheima.ai.repository.ChatHistoryRepository;

@RequiredArgsConstructor
@RestController
@RequestMapping("/ai")
public class CustomerServiceController {

    private final ChatClient serviceChatClient;
    private final ChatHistoryRepository chatHistoryRepository;

    @RequestMapping(value = "/service", produces = "text/html;charset=utf-8")
    public Flux<String> service(String prompt, String chatId) {
        // 1.保存会话id
        chatHistoryRepository.save("service", chatId);
        // 2.请求模型
        return serviceChatClient.prompt()
                .user(prompt)
                .advisors(a -> a.param(PromptConstants.CUSTOMER_SERVICE_SYSTEM, chatId))
                .stream()
                .content();
    }
}
```


通过这些，大模型不仅拥有了“小黑”的客服人设与记忆，还通过 `CourseTools` 获得了自主调用后端方法去检索课程的能力！



# RAG


## 概念介绍


纯粹从技术架构与底层原理的视角来剖析，RAG（Retrieval-Augmented Generation，检索增强生成）是目前解决大语言模型（LLM）局限性最成熟、最主流的技术方案。


我们可以将 RAG 拆解为**核心痛点、两大阶段（离线索引与在线推理）以及进阶优化维度**来深度理解：

  
### 一、 为什么大模型必须依赖 RAG？

大语言模型本质上是一个基于概率的“超级文本预测器”，它存在三个天生无法通过自身解决的痛点：

  

1. **知识的静态与滞后性**：大模型的训练数据有明确的截止日期（Knowledge Cutoff），无法获知昨天刚刚发生的新闻、公司昨晚更新的规章制度。
    
      
    
2. **私有数据的盲区**：大模型在公开互联网数据上训练，绝不可能读过你企业的内部合同、私有代码库、核心数据库或个人文档。
    
      
    
3. **幻觉（Hallucination）**：当大模型遇到自己不知道的事实或专业领域时，为了迎合用户，往往会“一本正经地胡说八道”。
    
      
    

**RAG 的核心思想**：把大模型的“通用生成能力”与外部的“精准检索能力”结合起来。用大白话讲，就是**开卷考试**——用户提问时，系统先去资料库里把正确的参考书翻出来，让大模型看着参考书来回答问题。

  

### 二、 RAG 的标准架构与工作流程

整个 RAG 系统严密地分为两个核心阶段：**离线索引阶段（Indexing）** 和 **在线检索与生成阶段（Retrieval & Generation）**。

  
#### 阶段一：离线数据准备（索引构建）

这一步的目标是将企业的私有文档变成计算机能高效检索的结构化数据：

  

1. **数据加载（Document Loading）**：
    
    从各种源头（PDF、Word、Markdown、HTML、数据库、Confluence 等）读取原始文本。
    
      
    
2. **文本切分（Text Splitting / Chunking）**：
    
    大模型的上下文窗口有限，且大段文本会稀释检索精度。因此需要将长文档切分成一个个较小的段落（称为 **Chunk**，通常为 200~500 个字符），并保留适当的重叠（Overlap）以防语义被切断。
    
      
    
3. **向量化（Embedding）**：
    
    利用嵌入模型（Embedding Model，如 OpenAI text-embedding-3、BGE 等），将切分好的文本段落转换成高维的数学向量（稠密向量）。**向量的本质是将文字的“语义”变成空间坐标**，语义相近的文本，其坐标距离也越近。
    
      
    
4. **向量数据库存储（Vector Database Storage）**：
    
    将生成的向量、原始文本片段以及元数据（如文档来源、标题）存入专用的向量数据库中（如 Milvus、Pinecone、Qdrant、Chroma、PgVector 等）。
    
      
    

#### 阶段二：在线检索与生成（核心推理链路）

当用户在前端输入一个问题时，系统会瞬间触发以下流水线：

  

1. **用户提问（Query）**：用户输入：“公司最新的年假报销标准是什么？”
    
      
    
2. **问题向量化（Query Embedding）**：将用户的问题同样通过嵌入模型转换为向量。
    
      
    
3. **相似度检索（Retrieval / Top-K Search）**：
    
    用问题的向量去向量数据库中进行高维空间检索（如计算余弦相似度），秒级捞出与这个问题**语义最相关**的 3 到 5 个文本片段（Chunks）。
    
      
    
4. **Prompt 拼装（Context Augmentation）**：
    
    系统将捞出的参考文档片段作为“上下文（Context）”，与用户的原始问题一起，按照特定的结构拼装成一个增强版的 Prompt。例如：
    
      
    
    > _“已知以下参考资料：\n [片段1: ...] \n [片段2: ...] \n\n 请严格根据上述参考资料回答用户问题：公司最新的年假报销标准是什么？如果资料中没有，请直接回答‘不知道’。”_
    > 
    >   
    
5. **大模型生成（Generation）**：
    
    大模型阅读这些精准的参考资料后，结合其强大的语言组织能力，生成严谨、有据可查的最终回答反馈给用户。
    
      
    

### 三、 进阶：如何打造一个高精度的生产级 RAG？

在实际工业界落地时，基础的 RAG（Naive RAG）往往会遇到“检索不准”、“漏掉关键信息”等问题，因此演进出了高阶的 RAG 优化策略：

  

1. **混合检索（Hybrid Search）**：
    
    单纯靠向量语义检索有时会漏掉精确的关键词（如专有名词、产品型号）。生产环境中通常会结合传统的**关键词检索（BM25）**与**向量检索**，取长补短。
    
      
    
2. **重排模型（Re-ranker）**：
    
    向量检索初筛时通常会捞出前 20 个片段（Top-20），然后通过一个计算成本更高但精度极高的交叉编码器（Cross-Encoder Re-ranker）对这 20 个片段进行重新打分排序，精选出最优质的的前 3 个送给大模型。
    
      
    
3. **查询重写（Query Rewriting）**：
    
    用户的提问往往过于口语化或含糊不清。系统可以先让大模型把用户的口语化提问改写成规范、信息密度更高的检索词，再去向量数据库中查找。
    
      
    

通过这套独立于任何编程语言和框架的底层架构，RAG 成功让大模型具备了处理企业级私有海量知识的安全边界与精确度。



## Spring AI 中的 RAG


在前面我们探讨了 RAG（检索增强生成）的底层理论之后，我们来看看 **Spring AI** 是如何将这一套复杂的流程进行优雅封装的。


在 Spring AI 中，RAG 并不是一堆散乱的 API 拼接，而是被抽象成了一套**开箱即用、高度契合 Spring 生态**的标准化组件。它将 RAG 拆解为两个核心部分：**离线数据加载与索引**、**在线检索与拦截增强**。

  

### 一、 Spring AI 中的 RAG 核心组件架构

Spring AI 针对 RAG 的各个环节，提供了极其清晰的接口抽象：

  

1. **`Document` 与 `DocumentReader`（文档加载与解析）**
    
      
    - 用于读取各种格式的源文件（PDF、文本、Markdown、JSON 等），并将其统一封装为 Spring AI 的标准 `Document` 对象（包含文本内容和元数据 Metadata）。
        
          
        
2. **`TextSplitter`（文本切分）**
    
      
    - 负责将长文本切分成适合向量化的小段落（Chunk），支持 Token 级别或字符级别的切分策略。
        
          
        
3. **`EmbeddingModel`（向量化模型）**
    
      
    - 统一了各大模型厂商的 Embedding 接口（如 OpenAI、Ollama 等），将文本转换成高维向量。
        
          
        
4. **`VectorStore`（向量数据库抽象）**
    
      
    - **Spring AI 的杀手锏之一**。它屏蔽了底层不同向量数据库（如 Milvus、PgVector、Chroma、Pinecone、Elasticsearch 等）的 API 差异。你只需要面向 `VectorStore` 编程，换数据库只需改个 Starter 和配置。
        
          
        
5. **`QuestionAnswerAdvisor`（运行时 RAG 核心拦截器）**
    
      
    - 这就是我们在前面学习 Advisor 体系时提到过的专用于 RAG 的拦截器。它在运行时自动拦截用户的提问，去 `VectorStore` 中检索相关文档，并自动拼装到 Prompt 中。
        
          
        

### 二、 核心实战：如何在 Spring AI 中实现 RAG？

在 Spring AI 中实现一个具备知识库问答能力的 RAG 应用，代码出奇的简洁。通常只需要完成**向量库初始化**和**在 `ChatClient` 中挂载 Advisor** 两步。

  
#### 1. 初始化与配置向量库（VectorStore）

Spring Boot 启动时，你可以将准备好的本地文档（比如放在 `classpath` 下的公司规章制度文档）加载并存入向量数据库中：

  
```Java
@Configuration
public class RagConfig {

    /**
     * 配置向量数据库（以内存向量库 SimpleVectorStore 或其他持久化向量库为例）
     */
    @Bean
    public VectorStore vectorStore(EmbeddingModel embeddingModel) {
        SimpleVectorStore vectorStore = new SimpleVectorStore(embeddingModel);
        
        // 如果有离线文档，可以在这里加载、切分并写入向量库
        // Resource resource = new ClassPathResource("company-policy.pdf");
        // DocumentReader reader = new PagePdfDocumentReader(resource);
        // List<Document> documents = reader.get();
        // TextSplitter splitter = new TokenTextSplitter();
        // vectorStore.add(splitter.split(documents));
        
        return vectorStore;
    }
}
```

#### 2. 在业务层通过 Advisor 开启 RAG 问答

在构建 `ChatClient` 或在发起请求时，直接引入 `QuestionAnswerAdvisor`。所有的检索、拼接、增强逻辑全部由框架在底层自动托管：



```Java
@RestController
@RequestMapping("/ai")
@RequiredArgsConstructor
public class ChatController {

    private final ChatClient chatClient;
    private final VectorStore vectorStore; // 注入刚才配置好的向量库

    @GetMapping("/rag-chat")
    public String ragChat(String prompt) {
        return chatClient.prompt()
                .user(prompt)
                // 关键：挂载 RAG 增强拦截器，它会自动去 vectorStore 检索最相关的文档并拼接到 Prompt 中
                .advisors(new QuestionAnswerAdvisor(vectorStore))
                .call()
                .content();
    }
}
```

### 三、 总结

Spring AI 极大地降低了企业落地 RAG 的门槛：

  

- **解耦存储**：通过 `VectorStore` 接口，让你无缝切换不同的向量数据库。
    
      
    
- **极简调用**：通过 `QuestionAnswerAdvisor`，把原本复杂的“检索 $\rightarrow$ 拼装 $\rightarrow$ 生成”三步流水线变成了一行链式调用。
    
      
    

这使得 Java 开发者可以像操作传统数据库一样，轻松为大模型外挂一个企业级的私有知识库！





# ChatPDF——向量模型


下面介绍开发 **ChatPDF（PDF 知识库问答）** 项目时，**向量模型（Embedding Model）** 的底层数学原理以及在 Spring AI 中的配置与使用步骤。

![[Pasted image 20261017170630.png]]

### 一、 核心原理：什么是向量模型（Embedding）？

在 RAG 场景中，计算机无法直接判断两段自然语言（如“我爱上班”和“工作使我快乐”）语义是否相似。**向量模型的作用就是把文本转换成高维空间中的数组（向量 Vector）**。

  

1. **语义几何化**：
    
      
    - 语义相似的文本（例如“我爱上班”、“上班真好”、“我爱工作”），在向量空间中的距离非常近。
        
          
        
    - 语义不同甚至相反的文本（例如“拒绝加班”、“我要躺平”），在向量空间中会被推到较远的位置。
        
          
        
2. **相似度计算公式**：
    
      
    - **欧氏距离（Euclidean Distance）**：距离数值越小，说明越相似。
        
          
        
    - **余弦相似度（Cosine Similarity）**：夹角余弦值越大（越接近 1），说明语义方向越一致，相似度越高。
        
          
        

### 二、 Spring AI 中的三步落地流程

在 Spring AI 中调用向量模型非常简单，只需**引入依赖 $\rightarrow$ 配置参数 $\rightarrow$ 注入组件调用**。


#### 第一步：引入依赖 (`pom.xml`)

引入 OpenAI 兼容协议的 Spring Boot Starter 依赖（包含 ChatModel 与 EmbeddingModel）：

  
```XML
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-openai-spring-boot-starter</artifactId>
</dependency>
```

#### 第二步：配置向量模型 (`application.yaml`)

在配置文件中，除了配置对话模型 `chat` 外，还需要增加 `embedding` 节点的配置，指定向量模型的名称和维度：


```YAML
spring:
  ai:
    openai:
      base-url: https://dashscope.aliyuncs.com/compatible-mode # 兼容 API 网关
      api-key: ${OPENAI_API_KEY}
      chat:
        options:
          model: qwen-max
          temperature: 0.8
      embedding:
        options:
          model: text-embedding-v3 # 指定使用的向量模型名称
          dimensions: 1024         # 输出的向量维度（如 1024 维）
```

#### 第三步：注入 `EmbeddingModel` 转化为向量 (`float[]`)

在 Java 代码中直接注入 `EmbeddingModel` 接口（Spring Boot 会根据 yaml 配置自动注入实现类，如 `OpenAiEmbeddingModel`），调用 `.embed(...)` 即可将字符串转化为浮点数数组：

  
```Java
package com.itheima.ai;

import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.ai.embedding.EmbeddingModel;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
class AiDemoApplicationTests {

    @Autowired
    private EmbeddingModel embeddingModel; // 注入 Spring AI 统一的向量模型接口

    @Test
    void testEmbedding() {
        String query = "我爱黑马程序员";
        
        // 将字符串文本转换为高维向量数组 (float[])
        float[] queryVector = embeddingModel.embed(query);
        
        System.out.println("向量长度（维度）: " + queryVector.length); // 输出 1024
        System.out.println("第一个维度坐标: " + queryVector[0]);
    }
}
```

### 三、 总结

通过 `EmbeddingModel`，我们成功把自然语言文本转换成了 **1024 维度的浮点型数组 (`float[]`)**。这个数组就是文本在数学空间里的“语义指纹”，也是后续存入向量数据库、进行精确文档检索的基础数据结构！



# ChatPDF——向量数据库和PDF处理

下面介绍如何在 Spring AI 中通过向量数据库（Vector Store）和 **PDF 读取器（PDF Document Reader）** 来构建企业级私有知识库。

![[Pasted image 20261017185458.png]]

 当企业积累了海量的私有文档（如几十万个 PDF 片段），每个片段都被转换成了上千维度的向量。传统的传统关系型数据库（如 MySQL）擅长做精准的等于、范围查询，但**完全无法高效支撑海量高维向量的相似度计算（如欧氏距离、余弦相似度检索）**。为了解决大规模向量的秒级检索问题，专用的向量数据库应运而生。


同时，企业真正的核心知识（如规章制度、合同、产品手册）通常都保存在复杂的 **PDF、Word 等非结构化文档**中。大模型无法直接读取原始的二进制文件，且其受限于单次输入的字数上限（上下文窗口），无法直接吃下一整本书。因此，必须出现专门的文档解析与切片工具，把长文档揉碎、格式化并规范输出，才能喂给向量模型和知识库。

### 一、 整体架构流程

在 ChatPDF 场景中，整个系统由**离线构建**与**在线问答**两条链路组成：

  

1. **离线构建链路（PDF $\rightarrow$ 向量库）**：
    
      
    - **读取与切分**：加载原始 PDF 文档，按页或按段落切分为文本片段（Chunks）。
        
          
        
    - **向量化**：调用向量模型（Embedding Model）将文本片段转化为高维向量。
        
          
        
    - **入库**：将向量及其对应的文本内容存入向量数据库中。
        
          
        
2. **在线问答链路（提问 $\rightarrow$ 检索增强 $\rightarrow$ 回答）**：
    
      
    - 用户提出问题（如：“最新政策里关于报销的规定是什么？”）。
        
          
        
    - 系统将问题转为向量，去向量数据库中做相似度检索，精准捞出相关 PDF 段落。
        
          
        
    - 拼接检索到的 PDF 内容作为 Context 给大模型，大模型基于文档内容精准生成回答。
        
          
        

### 二、 向量数据库配置（以 Redis Vector Store 为例）

Spring AI 对主流向量数据库（Redis、PgVector、Milvus、Chroma 等）提供了统一的接口封装 `VectorStore`。

  

#### 1. 引入依赖 (`pom.xml`)

假如用 Redis，引入其依赖

```XML
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-redis-store-spring-boot-starter</artifactId>
</dependency>
```

  

#### 2. 配置文件 (`application.yaml`)


```YAML
spring:
  ai:
    vectorstore:
      redis:
        index: spring_ai_index     # 向量库索引名
        initialize-schema: true     # 是否自动初始化向量库索引结构
        prefix: "doc:"              # 向量库存储 key 前缀
  data:
    redis:
      host: 192.168.150.101          # Redis 服务地址
```

若使用简单的单机或内存测试，也可以使用 `SimpleVectorStore` 构建 Bean


```Java
@Bean
public VectorStore vectorStore(OpenAiEmbeddingModel model) {
    return SimpleVectorStore.builder(model).build();
}
```

### 三、 PDF 处理与写入向量库

Spring AI 专门提供了 `spring-ai-pdf-document-reader` 来解析与处理 PDF 文件。

#### 1. 引入 PDF 依赖 (`pom.xml`)


```XML
<dependency>
    <groupId>org.springframework.ai</groupId>
    <artifactId>spring-ai-pdf-document-reader</artifactId>
</dependency>
```

  
#### 2. 读取 PDF 并写入向量库的核心代码实现


```Java
package com.itheima.ai.service;

import org.springframework.ai.document.Document;
import org.springframework.ai.reader.pdf.PagePdfDocumentReader;
import org.springframework.ai.reader.pdf.config.PdfDocumentReaderConfig;
import org.springframework.ai.transformer.splitter.ExtractedTextFormatter;
import org.springframework.ai.vectorstore.VectorStore;
import org.springframework.core.io.Resource;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class PdfIndexingService {

    private final VectorStore vectorStore;

    public PdfIndexingService(VectorStore vectorStore) {
        this.vectorStore = vectorStore;
    }

    /**
     * 读取 PDF 文件并存入向量数据库
     * @param resource PDF 文件资源（如 ClassPathResource 或 FileSystemResource）
     */
    public void writeFileToVectorStore(Resource resource) {
        // 1. 创建 PagePdfDocumentReader 读取 PDF 内容
        PagePdfDocumentReader reader = new PagePdfDocumentReader(
                resource,
                PdfDocumentReaderConfig.builder()
                        .withPageExtractedTextFormatter(ExtractedTextFormatter.defaults())
                        .withPagesPerDocument(1) // 设置每 1 页 PDF 作为一个 Document 片段
                        .build()
        );

        // 2. 读取 PDF 文件为 Spring AI 标准的 Document 格式列表
        List<Document> documents = reader.read();

        // 3. 将文档自动转化为向量并写入向量数据库
        vectorStore.add(documents);
        
        System.out.println(">> 成功将 PDF 文档写入向量数据库，切分片段数: " + documents.size());
    }
}
```

### 四、 总结与统一接口

借助 Spring AI 抽象出来的核心接口 `VectorStore`，读写向量数据仅需两个最核心的方法：

  

- **`vectorStore.add(documents)`**：自动调用 Embedding 模型将文本向量化并写入存储库。
    
      
    
- **`vectorStore.similaritySearch(text)`**：根据传入的检索文本，自动转换为向量并在数据库中做相似度计算，返回最匹配的文档片段列表。
    
      
    


### 五、 测试代码

#### 1. 准备工作：测试用的 PDF 文件

在 `src/test/resources` 目录下放置一个名为 `test-knowledge.pdf` 的测试文件（例如：包含公司规章制度、培训课程或产品介绍的 PDF）。


#### 2. 完整测试代码：`RagVectorStoreTests.java`


```Java
package com.itheima.ai;

import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;
import org.springframework.ai.document.Document;
import org.springframework.ai.reader.pdf.PagePdfDocumentReader;
import org.springframework.ai.reader.pdf.config.PdfDocumentReaderConfig;
import org.springframework.ai.transformer.splitter.ExtractedTextFormatter;
import org.springframework.ai.vectorstore.SearchRequest;
import org.springframework.ai.vectorstore.VectorStore;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.core.io.ClassPathResource;
import org.springframework.core.io.Resource;

import java.util.List;

@SpringBootTest
public class RagVectorStoreTests {

    @Autowired
    private VectorStore vectorStore; // 注入 Spring AI 统一的向量数据库组件（如 SimpleVectorStore 或 RedisVectorStore）

    /**
     * 测试 1：读取 PDF 文档，切分并转化为向量写入向量数据库
     */
    @Test
    @DisplayName("测试读取PDF并写入向量数据库")
    void testPdfWriteToVectorStore() {
        // 1. 加载 classpath 下的 PDF 资源文件
        Resource pdfResource = new ClassPathResource("test-knowledge.pdf");

        // 2. 创建 PagePdfDocumentReader 读取并配置解析规则
        PagePdfDocumentReader pdfReader = new PagePdfDocumentReader(
                pdfResource,
                PdfDocumentReaderConfig.builder()
                        .withPageExtractedTextFormatter(ExtractedTextFormatter.defaults())
                        .withPagesPerDocument(1) // 设置每 1 页 PDF 作为一个独立的 Document 片段
                        .build()
        );

        // 3. 将 PDF 解析读取为 Spring AI 的 List<Document> 格式
        List<Document> documents = pdfReader.read();
        System.out.println(">> [PDF读取完成] 解析到的文档片段数量: " + documents.size());

        // 4. 将 Document 写入向量数据库
        // 底层会调用 EmbeddingModel 自动将每个 Document 的文本转为 1024 维的 float[] 向量并存入数据库
        vectorStore.add(documents);
        System.out.println(">> [向量化入库成功] 所有 PDF 内容已写入向量数据库！");
    }

    /**
     * 测试 2：在向量数据库中进行相似度语义检索（Similarity Search）
     */
    @Test
    @DisplayName("测试向量数据库相似度检索")
    void testSimilaritySearch() {
        // 检索关键词/提问文本
        String queryText = "请问公司关于请假的规定是怎样的？";

        // 方式 1：最简检索，直接根据问题找到相似度最高的文档片段
        List<Document> results = vectorStore.similaritySearch(queryText);

        System.out.println(">> [相似度检索结果] 命中片段数量: " + results.size());
        for (int i = 0; i < results.size(); i++) {
            Document doc = results.get(i);
            System.out.printf("--- 匹配片段 %d ---\n", i + 1);
            System.out.println("片段内容: " + doc.getContent());
            System.out.println("元数据信息: " + doc.getMetadata());
        }

        // 方式 2：高级检索，通过 SearchRequest 限制返回前 Top-K 个结果及相似度阈值
        SearchRequest request = SearchRequest.builder()
                .query(queryText)
                .topK(3)           // 只取相关性最高的前 3 个片段
                .similarityThreshold(0.7) // 设定相似度分数阈值（如余弦相似度大于 0.7）
                .build();

        List<Document> filteredResults = vectorStore.similaritySearch(request);
        System.out.println(">> [高级过滤检索结果] 阈值过滤后的片段数量: " + filteredResults.size());
    }
}
```

#### 3. 测试代码执行流程解密

1. **执行 `testPdfWriteToVectorStore()`**：
    
      
    - `PagePdfDocumentReader` 解析二进制 PDF 文件并转为标准的字符串文本片段（`Document`）。
        
          
        
    - `vectorStore.add(documents)` 被触发后，框架内部自动调用配置好的 `EmbeddingModel` 批量计算每个片段的向量，然后连同文本存入向量库（如内存库 `SimpleVectorStore` 或分布式向量库 `RedisVectorStore`）。
        
          
        
2. **执行 `testSimilaritySearch()`**：
    
      
    - 输入口语化问题（例如：“公司关于请假的规定”）。
        
          
        
    - 框架自动将此提问转化为向量后去向量库进行**余弦相似度计算**，准确提取出包含了请假细则的 PDF 页码或段落。





# ChatPDF——代码实现



ChatPDF 的三大核心功能为：

  

1. **文件上传并导入向量库**：解析上传的 PDF 文件，将其切分为 `Document` 片段，提取元数据（如 `file_name`），存入 `VectorStore`。
    
      
    
2. **文件下载**：将保存在本地或服务器上的原始 PDF 文件下载给客户端。
    
      
    
3. **AI 针对具体 PDF 进行对话**：利用 `QuestionAnswerAdvisor` 拦截器进行向量检索，并通过元数据过滤表达式（`FILTER_EXPRESSION`），精确限定只检索当前正在对话的某一个 PDF 文件（如 `file_name == 'hello.pdf'`）。
    
      
    

  ![[Pasted image 20261017201921.png]]

### 一、 核心配置类：配置 ChatPDF 专属的 `ChatClient`

在配置类中配置 `QuestionAnswerAdvisor` 拦截器，设置相似度阈值和检索片段数量（Top-K）：


```Java
package com.itheima.ai.config;

import org.springframework.ai.chat.client.ChatClient;
import org.springframework.ai.chat.client.advisor.MessageChatMemoryAdvisor;
import org.springframework.ai.chat.client.advisor.QuestionAnswerAdvisor;
import org.springframework.ai.chat.client.advisor.SimpleLoggerAdvisor;
import org.springframework.ai.chat.memory.ChatMemory;
import org.springframework.ai.chat.memory.InMemoryChatMemory;
import org.springframework.ai.openai.OpenAiChatModel;
import org.springframework.ai.vectorstore.SearchRequest;
import org.springframework.ai.vectorstore.VectorStore;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class ChatPdfConfig {

    @Bean
    public ChatMemory chatMemory() {
        return new InMemoryChatMemory();
    }

    /**
     * 配置用于 ChatPDF 的 ChatClient（整合记忆、日志和 RAG Advisor）
     */
    @Bean
    public ChatClient pdfChatClient(
            OpenAiChatModel model,
            ChatMemory chatMemory,
            VectorStore vectorStore) {

        return ChatClient.builder(model)
                .defaultAdvisors(
                        new MessageChatMemoryAdvisor(chatMemory), // 会话记忆拦截器
                        new SimpleLoggerAdvisor(),                // 日志拦截器
                        new QuestionAnswerAdvisor(                 // RAG 知识库问答拦截器
                                vectorStore,
                                SearchRequest.builder()
                                        .similarityThreshold(0.6d) // 设定相似度阈值
                                        .topK(2)                   // 返回相关性最高的前2个文档片段
                                        .build()
                        )
                )
                .build();
    }
}
```

### 二、 业务服务层：`ChatPdfService.java`

负责 PDF 文件的保存、解析、写入向量库，以及文件下载：


```Java
package com.itheima.ai.service;

import lombok.RequiredArgsConstructor;
import org.springframework.ai.document.Document;
import org.springframework.ai.reader.pdf.PagePdfDocumentReader;
import org.springframework.ai.reader.pdf.config.PdfDocumentReaderConfig;
import org.springframework.ai.transformer.splitter.ExtractedTextFormatter;
import org.springframework.ai.vectorstore.VectorStore;
import org.springframework.core.io.FileSystemResource;
import org.springframework.core.io.Resource;
import org.springframework.stereotype.Service;
import org.springframework.web.multipart.MultipartFile;

import java.io.File;
import java.io.IOException;
import java.nio.file.Path;
import java.nio.file.Paths;
import java.util.List;

@Service
@RequiredArgsConstructor
public class ChatPdfService {

    private final VectorStore vectorStore;
    
    // PDF 文件本地保存路径
    private final String uploadDirPath = "uploads/pdf/";

    /**
     * 1. 导入上传的 PDF 文件到向量数据库
     */
    public String uploadAndIndexPdf(MultipartFile file) throws IOException {
        String fileName = file.getOriginalFilename();
        
        // 确保存储目录存在
        File dir = new File(uploadDirPath);
        if (!dir.exists()) {
            dir.mkdirs();
        }

        // 将文件保存到本地磁盘
        File targetFile = new File(dir, fileName);
        file.transferTo(targetFile);

        // 使用 PagePdfDocumentReader 解析 PDF
        Resource resource = new FileSystemResource(targetFile);
        PagePdfDocumentReader pdfReader = new PagePdfDocumentReader(
                resource,
                PdfDocumentReaderConfig.builder()
                        .withPageExtractedTextFormatter(ExtractedTextFormatter.defaults())
                        .withPagesPerDocument(1) // 每1页PDF作为一个独立的Document片段
                        .build()
        );

        // 读取 PDF 为 Document 列表
        List<Document> documents = pdfReader.read();

        // 为每个文档片段的 Metadata 动态写入 file_name 标识（用于后续精准条件过滤）
        for (Document doc : documents) {
            doc.getMetadata().put("file_name", fileName);
        }

        // 写入向量数据库（自动调用 EmbeddingModel 存入向量）
        vectorStore.add(documents);

        return fileName;
    }

    /**
     * 2. 获取用于下载的本地 PDF 文件资源
     */
    public Resource getPdfResource(String fileName) {
        Path filePath = Paths.get(uploadDirPath).resolve(fileName).normalize();
        File file = filePath.toFile();
        if (file.exists()) {
            return new FileSystemResource(file);
        } else {
            throw new RuntimeException("文件不存在：" + fileName);
        }
    }
}
```

### 三、 控制层：`ChatPdfController.java`

包含文件上传、文件下载以及**带文件隔离条件的流式对话接口**：


```Java
package com.itheima.ai.controller;

import com.itheima.ai.service.ChatPdfService;
import lombok.RequiredArgsConstructor;
import org.springframework.ai.chat.client.ChatClient;
import org.springframework.ai.chat.client.advisor.AbstractChatMemoryAdvisor;
import org.springframework.ai.chat.client.advisor.QuestionAnswerAdvisor;
import org.springframework.core.io.Resource;
import org.springframework.http.HttpHeaders;
import org.springframework.http.MediaType;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.multipart.MultipartFile;
import reactor.core.publisher.Flux;

import java.io.IOException;

@RestController
@RequestMapping("/ai/pdf")
@RequiredArgsConstructor
public class ChatPdfController {

    private final ChatClient pdfChatClient;
    private final ChatPdfService chatPdfService;

    /**
     * 功能 1：文件上传并导入向量库
     */
    @PostMapping("/upload")
    public ResponseEntity<String> uploadPdf(@RequestParam("file") MultipartFile file) {
        try {
            String fileName = chatPdfService.uploadAndIndexPdf(file);
            return ResponseEntity.ok("文件导入向量库成功：" + fileName);
        } catch (IOException e) {
            return ResponseEntity.status(500).body("文件导入失败：" + e.getMessage());
        }
    }

    /**
     * 功能 2：文件下载
     */
    @GetMapping("/download/{fileName}")
    public ResponseEntity<Resource> downloadPdf(@PathVariable String fileName) {
        Resource resource = chatPdfService.getPdfResource(fileName);
        return ResponseEntity.ok()
                .contentType(MediaType.APPLICATION_PDF)
                .header(HttpHeaders.CONTENT_DISPOSITION, "attachment; filename=\"" + fileName + "\"")
                .body(resource);
    }

    /**
     * 功能 3：AI 针对具体 PDF 进行问答（图四核心代码）
     * 
     * @param prompt 用户的问题
     * @param chatId 会话 ID（用于维持多轮对话上下文）
     * @param fileName 当前对话指定的 PDF 文件名
     */
    @GetMapping(value = "/chat", produces = "text/html;charset=utf-8")
    public Flux<String> chat(
            @RequestParam String prompt,
            @RequestParam String chatId,
            @RequestParam String fileName) {

        return pdfChatClient.prompt(prompt)
                // 1. 动态传入会话 ID，绑定上下文记忆
                .advisors(a -> a.param(AbstractChatMemoryAdvisor.CHAT_MEMORY_CONVERSATION_ID_KEY, chatId))
                // 2. 关键：传入 RAG 过滤表达式，限定检索只在当前 file_name 对应的 PDF 文档片段中进行
                .advisors(a -> a.param(
                        QuestionAnswerAdvisor.FILTER_EXPRESSION,
                        "file_name == '" + fileName + "'"
                ))
                .stream()
                .content();
    }
}
```

### 四、 核心技术点总结

1. **精确文档隔离（`FILTER_EXPRESSION`）**：在真实应用中，向量数据库里存有海量不同用户的 PDF 文件。通过在 `QuestionAnswerAdvisor` 中传入 `file_name == 'xxx.pdf'` 表达式，就能实现只针对“当前打开的这本 PDF”进行检索。
    
      
    
2. **多轮对话与知识库缝合**：通过挂载 `MessageChatMemoryAdvisor` 和 `QuestionAnswerAdvisor`，使大模型既能**记住上文说过的话**，又能**只依据当前 PDF 中的内容严谨回答**。




# 多模态


### 一、 多模态对话的概念与引入原因

#### 1. 什么是多模态（Multi-Modality）？

传统的 AI 对话应用大都局限于**纯文本**交互（Text-to-Text）。而**多模态**是指大模型不仅能理解文字，还能同时处理和理解**图像、音频、视频**等多种不同类型的数据载体。

  

#### 2. 为什么需要多模态？

在真实的业务场景中（如智能客服、ChatPDF、Bug 诊断等），用户往往无法完全用文字描述清楚诉求。例如：

  

- 用户在使用系统时遇到了报错，截图发给客服：“请问这个红色的异常怎么解决？”
    
      
    
- 学员在看教材时，拍下一张图问：“这个架构图里的网关层是怎么流转的？”
    
      
    

如果 AI 不具备多模态能力，就无法“看懂”用户上传的图片。因此，我们需要引入支持多模态的底层大模型（如通义千问的多模态版本 `qwen-omni-turbo`），并通过代码将用户上传的图片或文件转化为 AI 能够理解的 `Media` 对象，无缝塞进提示词中。

  

### 二、 代码实现：让对话机器人支持多模态图片上传

我们需要分两步来改造代码：**第一步切换支持多模态的模型配置**，**第二步改造 Controller 以支持文件上传和多模态组装**。

  

#### 1. 修改配置类：切换为多模态模型

为了支持图像等多模态输入，我们需要在构建 `ChatClient` 时，通过 `.defaultOptions()` 将大模型显式指定为支持多模态的型号（例如 `qwen-omni-turbo`）。



```Java
package com.itheima.ai.config;

import org.springframework.ai.chat.client.ChatClient;
import org.springframework.ai.chat.client.advisor.MessageChatMemoryAdvisor;
import org.springframework.ai.chat.client.advisor.SimpleLoggerAdvisor;
import org.springframework.ai.chat.memory.ChatMemory;
import org.springframework.ai.chat.memory.InMemoryChatMemory;
import org.springframework.ai.openai.OpenAiChatModel;
import org.springframework.ai.openai.api.OpenAiApi;
import org.springframework.ai.openai.options.OpenAiChatOptions;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class AiConfig {

    @Bean
    public ChatMemory chatMemory() {
        return new InMemoryChatMemory();
    }

    @Bean
    public ChatClient chatClient(OpenAiChatModel model, ChatMemory chatMemory) {
        return ChatClient.builder(model)
                // 关键：通过 defaultOptions 切换为支持多模态的模型（如 qwen-omni-turbo）
                .defaultOptions(
                        OpenAiChatOptions.builder()
                                .model("qwen-omni-turbo")
                                .build()
                )
                .defaultSystem("您是一家名为“黑马程序员”的职业教育公司的客户聊天助手，你的名字叫小黑。请以友好、乐于助人和愉快的方式解答用户的各种问题。")
                .defaultAdvisors(
                        new SimpleLoggerAdvisor(),               // 日志拦截器
                        new MessageChatMemoryAdvisor(chatMemory) // 会话记忆拦截器
                )
                .build();
    }
}
```

#### 2. 修改 Controller：支持文件上传与多模态组装

在 Controller 中，我们需要接收前端传来的 `List<MultipartFile> files`。如果用户上传了图片，则将其转换为 Spring AI 的 `Media` 数组，并通过 `.user(c -> c.text(prompt).media(medias))` 动态发送给大模型。



```Java
package com.itheima.ai.controller;

import lombok.RequiredArgsConstructor;
import org.springframework.ai.chat.client.ChatClient;
import org.springframework.ai.model.Media;
import org.springframework.ai.util.MimeTypeUtils;
import org.springframework.util.MimeType;
import org.springframework.web.bind.annotation.*;
import org.springframework.web.multipart.MultipartFile;
import reactor.core.publisher.Flux;

import java.util.List;
import java.util.Objects;

import static org.springframework.ai.chat.client.advisor.AbstractChatMemoryAdvisor.CHAT_MEMORY_CONVERSATION_ID_KEY;

@RestController
@RequestMapping("/ai")
@RequiredArgsConstructor
@CrossOrigin("*")
public class ChatController {

    private final ChatClient chatClient;

    /**
     * 统一对话接口（同时支持纯文本对话 与 多模态图片问答）
     * 
     * @param files  上传的文件列表（可选）
     * @param prompt 用户的文本提示词
     * @param chatId 会话唯一ID
     */
    @GetMapping(value = "/chat", produces = "text/html;charset=utf-8")
    public Flux<String> chat(
            @RequestParam(value = "files", required = false) List<MultipartFile> files,
            @RequestParam(defaultValue = "讲个笑话") String prompt,
            @RequestParam String chatId) {

        // 1. 判断用户是否上传了文件/图片
        if (files != null && !files.isEmpty()) {
            return multiModalityChat(chatId, prompt, files); // 执行多模态对话
        } else {
            return textChat(chatId, prompt);                 // 执行纯文本对话
        }
    }

    /**
     * 多模态对话私有方法（处理带图片/文件的请求）
     */
    private Flux<String> multiModalityChat(String chatId, String prompt, List<MultipartFile> files) {
        // 将上传的 MultipartFile 转换为 Spring AI 的 Media 数组
        Media[] medias = files.stream()
                .map(file -> new Media(
                        MimeType.valueOf(Objects.requireNonNull(file.getContentType())),
                        file.getResource()
                ))
                .toArray(Media[]::new);

        return chatClient.prompt()
                // 使用 Lambda 形式同时传入文本 Prompt 和多媒体 Media 对象
                .user(c -> c.text(prompt).media(medias))
                .advisors(a -> a.param(CHAT_MEMORY_CONVERSATION_ID_KEY, chatId))
                .stream()
                .content();
    }

    /**
     * 纯文本对话私有方法
     */
    private Flux<String> textChat(String chatId, String prompt) {
        return chatClient.prompt()
                .user(prompt)
                .advisors(a -> a.param(CHAT_MEMORY_CONVERSATION_ID_KEY, chatId))
                .stream()
                .content();
    }
}
```

### 三、 核心代码亮点解析

1. **动态模型复用**：通过 `.defaultOptions(...)` 可以在不破坏全局其他业务的前提下，精准为该 `ChatClient` 切换到多模态大模型（`qwen-omni-turbo`）。
    
      
    
2. **多媒体适配器 (`Media`)**：Spring AI 将底层的不同文件类型统一封装为 `Media` 对象，通过 `file.getContentType()` 自动识别 MIME 类型，极大地简化了多模态输入的开发难度。
    
      
    
3. **链式流式组合**：通过 `.user(c -> c.text(prompt).media(medias))`，文字与图片在同一个用户输入槽位中完美结合，并继续向下兼容原有的内存 Advisor 和流式打字机输出（SSE）。