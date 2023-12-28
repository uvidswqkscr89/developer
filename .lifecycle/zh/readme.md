# 🐣 smol 开发者

<a href="https://app.e2b.dev/agent/smol-developer" target="_blank" rel="noopener noreferrer">
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://app.e2b.dev/api/badge_light">
  <img alt="在 e2b 上部署代理按钮" src="https://app.e2b.dev/api/badge"/>
</picture>
</a>
<a href="https://github.com/modal-labs/devlooper"><img src="https://github.com/smol-ai/developer/assets/6764957/6af16d37-2494-4722-b3a2-6fc91c005451"></img>
</a>
<a href="https://twitter.com/morph_labs/status/1689321673151979536"><img src="https://avatars.githubusercontent.com/u/136536927?s=40&v=4" alt="Morph"></img> Morph
</a>


***以人为本且连贯的整个程序综合***，也就是您自己的个人初级开发人员

> [构建构建事物的事物！](https://twitter.com/swyx/status/1657578738345979905)适合各种情况下每个开发人员的 `smol dev`

这是一个“初级开发人员”代理（又名 smol dev），它可以：

1. 一旦您给出产品规格，就会为您搭建整个代码库
2. 为您提供了在您自己的应用程序中拥有 smol 开发人员的基本构建块。

与其制作和维护特定、固定的一次性启动器（如 `create-react-app` 或 `create-nextjs-app`），不如制作一个基本的、可定制的启动器，就像 [`create-anything-app`](https://news.ycombinator.com/item?id=35942352) 一样，您可以在其中与您的 smol dev 一起紧密循环开发。

在[成功的初始 v0 发布](https://twitter.com/swyx/status/1657578738345979905)之后，smol 开发人员被重写为**更加精简**，并且可以从库中导入！

## 基本用法

### 在 Git Repo 模式下

```bash
# 安装
git clone https://github.com/smol-ai/developer.git
cd developer
poetry install # 安装依赖项。如果需要，可以使用 pip install poetry 安装 poetry

# 运行
python main.py "一个 HTML/JS/CSS 井字棋游戏" # 默认使用 gpt-4-0613
# python main.py "一个 HTML/JS/CSS 井字棋游戏" --model=gpt-3.5-turbo-0613

# 其他命令行标志
python main.py --prompt prompt.md # 对于较长的提示，请将其移动到 markdown 文件中
python main.py --prompt prompt.md --debug True # 用于调试
```

<details>
  <summary>
这样您就可以以人在循环中开发应用程序，就像最初版本的 smol 开发人员一样。
  </summary>


<p align="center">
  <img height=200 src="https://pbs.twimg.com/media/FwEzVCcaMAE7t4h?format=jpg&name=large" />
</p>


*通过提示进行工程，而不是通过提示进行工程*

`prompt.md` 中的演示示例展示了启用 AI 的潜力，但仍然坚定地以人类开发人员为中心的工作流程：

- 人类为他们想要构建的应用程序编写基本提示
- `main.py` 生成代码
- 人类运行/阅读代码
- 人类可以：
  - 在发现提示中未指定的部分时，只需添加到提示中
  - 手动运行代码并识别错误
  - *将错误粘贴到提示中*，就像他们提交 GitHub 问题一样
  - 如需额外帮助，他们可以使用 `debugger.py`，它会读取整个代码库以提出具体的代码更改建议

循环直到达到满意的结果。请注意，只有在 AI 增加价值时才使用它 - 一旦它妨碍了您，只需从您的 smol 初级开发人员手中接管代码库，无需大惊小怪，也不会伤害感情。 (*我们也可以让 smol-dev 接管现有的代码库并引导自己的提示... 但这是未来的方向*)

</details>


通过这种方式，您可以使用此存储库的克隆来原型/开发您的应用程序。

### 在 Library 模式下

这是 smol Developer v1 中的新功能！将 smol 开发人员添加到您自己的项目中！

```bash
pip install smol_dev
```

在这里，您可以将 `main.py` 的内容视为我们的“文档”，了解如何在您自己的应用程序中使用这些函数和提示：

```python
from smol_dev.prompts import plan, specify_file_paths, generate_code_sync

prompt = "一个 HTML/JS/CSS 井字棋游戏"

shared_deps = plan(prompt) # 返回表示编码计划的长字符串

# 如果需要，可以对 shared_deps 计划进行处理，例如要求用户确认/编辑并在循环中迭代

file_paths = specify_file_paths(prompt, shared_deps) # 返回一个字符串数组，表示根据您的提示和 shared_deps 需要编写的文件名。依赖于 OpenAI 的新函数调用 API 来保证 JSON。

# 如果需要，可以对文件路径进行处理，例如显示计划

# 遍历 file_paths 数组，并为每个文件生成代码
for file_path in file_paths:
    code = generate_code_sync(prompt, shared_deps, file_path) # 生成每个文件的源代码

    # 对文件的源代码进行处理，例如写入磁盘或在 UI 中显示
    # 还有一个异步的 `generate_code()` 版本
```

### 在 API 模式下（通过 [Agent Protocol](https://github.com/e2b-dev/agent-protocol)）
要启动服务器，请运行：
```bash
poetry run api
```
或者
```bash
python smol_dev/api.py
```

然后，您可以使用以下命令之一调用 API：

要**创建任务**，请运行：
```bash
curl --request POST \
  --url http://localhost:8000/agent/tasks \
  --header 'Content-Type: application/json' \
  --data '{
	"input": "Write simple script in Python. It should write '\''Hello world!'\'' to hi.txt"
}'
```

您将获得如下响应：
```json
{"input":"Write simple script in Python. It should write 'Hello world!' to hi.txt","task_id":"d2c4e543-ae08-4a97-9ac5-5f9a4459cb19","artifacts":[]}
```

然后，要**执行任务的一个步骤**，请复制从上一个请求中获取的 `task_id` 并运行：

```bash
curl --request POST \
  --url http://localhost:8000/agent/tasks/<task-id>/steps
```

或者，您可以使用[Python 客户端库](https://github.com/e2b-dev/agent-protocol/tree/main/agent_client/python)：

```python
from agent_protocol_client import AgentApi, ApiClient, TaskRequestBody

...

prompt = "Write simple script in Python. It should write 'Hello world!' to hi.txt"

async with ApiClient() as api_client:
    # 创建 API 类的实例
    api_instance = AgentApi(api_client)
    task_request_body = TaskRequestBody(input=prompt)

    task = await api_instance.create_agent_task(
        task_request_body=task_request_body
    )
    task_id = task.task_id
    response = await api_instance.execute_agent_task_step(task_id=task_id)

...

```

## 示例/提示库

- [6 分钟视频演示](https://youtu.be/UCo7YeTy-aE) -（抱歉，音频加快了，我们正在针对 Twitter 进行优化，通话不好）
  - 这是最初的 smol 开发人员演示 - 从提示符到完整的 chrome 扩展，请求和存储 apikey，生成弹出窗口，读取和传输页面内容，并使用 Anthropic Claude 有用地总结任何网站，根据输入的长度切换模型到 100k 的模型
  - 提示位于[prompt.md](https://github.com/smol-ai/developer/blob/main/prompt.md)中，它输出[/exampleChromeExtension](https://github.com/smol-ai/developer/tree/main/examples/exampleChromeExtension)
- `smol-plugin` - 提示 ChatGPT 插件（[tweet](https://twitter.com/ultrasoundchad/status/1659366507409985536?s=20)，[fork](https://github.com/gmchad/smol-plugin)）

  <img src="https://github.com/smol-ai/developer/assets/6764957/6ffaac3b-5d90-460a-a590-c8a8c004bd36" height=200 />



```
- [提示转化为 Pokemon 应用](https://twitter.com/RobertCaracaus/status/1659312419485761536?s=20)

  <img src="https://github.com/smol-ai/developer/assets/6764957/15fa189a-3f52-4618-ac8e-2a77b6500264" height=200 />


- [政治竞选 CRM 程序示例](https://github.com/smol-ai/developer/pull/22/files)
- [使用 GPT-4 创建 VSCode 扩展的经验教训](https://bit.kevinslin.com/p/leveraging-gpt-4-to-automate-the)（也在 [HN](https://news.ycombinator.com/item?id=36071342) 上）
- [7 分钟视频：Smol AI Developer - 使用单一提示构建整个代码库](https://www.youtube.com/watch?v=DzRoYc2UGKI) 从提示生成完整的工作 OpenAI CLI Python 应用程序

  <img src="https://github.com/smol-ai/developer/assets/6764957/e80058f1-ea9c-42dd-87ff-004b61f08f2e" height=200 />


- [12 分钟视频：SMOL AI - 一键使用 AGI 开发大型应用程序](https://www.youtube.com/watch?v=zsxyqz6SYp8) 在 40 分钟内并且只花费 9 美元就搭建了一个出人意料的复杂的 React/Node/MongoDB 全栈应用程序

  <img src="https://github.com/smol-ai/developer/assets/6764957/c51f9f8c-021d-446a-b44d-7a6f48e64550" height=200 />


我正在积极寻找更多示例，请提交您的示例！

抱歉例子不多，我知道这可能会让人感到沮丧，但我还没准备好迎接这么多人的关注，哈哈

## 主要分支/替代品

请提交替代实现，并在其他技术栈上部署策略！

- **JS/TS**：[smol-dev-js](https://github.com/PicoCreator/smol-dev-js) 是 smol-dev 的纯 JS 变体，允许通过提示进行更小的增量更改（如果你不想做整个 spec2code 事情），可以将其实时插入任何项目中（无论好坏）
- **C#/Dotnet**：[smol-ai-dotnet](https://github.com/colhountech/smol-ai-dotnet) 用 C# 实现！
- **Golang**：[smol-dev-go](https://github.com/tmc/smol-dev-go) 用 Go 语言实现
- [smol-plugin](https://github.com/gmchad/smol-plugin) 通过在 smol-developer 风格的 markdown 中指定 API，自动生成 @openai 插件
- 你的分支在这里！

### 创新和洞察

> 请订阅 https://latent.space/ 获取更完整的文章以及洞察和反思

- **Markdown 就是你所需要的** - Markdown 是提示整个程序合成的完美方式，因为它很容易混合英语和代码（无论是 `variable_names` 或完整的 ``` 代码块）
  - 事实证明，你可以在代码中指定提示，gpt4 会严格遵守
- **复制和粘贴编程**
  - 通过粘贴 `curl` 输入和输出来教程序如何围绕新 API 编码（Anthropic 的 API 是在 GPT-3 知识截止之后的）
  - 将错误消息粘贴到提示中，并含糊其辞地告诉程序你希望如何处理。这感觉有点像“日志驱动编程”。
- **通过 `cat` 整个代码库和你的错误消息来调试，并得到具体的修复建议** - 特别令人愉快！
- **整个程序一致性的技巧** - 我们选择的示例用例，Chrome 扩展，在文件之间有很多间接依赖。任何交叉依赖的幻觉都会导致整个程序出错。
  - 我们通过添加一个中间步骤来解决这个问题，要求 GPT 考虑 `shared_dependencies.md`，然后坚持在生成每个文件时使用它。这基本上意味着 GPT 能够与自己对话......
  - ...但它还不完美。`shared_dependencies.md` 有时并不全面理解文件之间的硬依赖。所以我们通过在提示中指定一个特定的 `name` 来解决它。一开始感觉不太对劲，但它确实有效，最终这只是清晰明确的沟通。
  - 请参阅 `prompt.md` 了解 SOTA smol-dev 提示
- **对不熟悉的 API，活化能较低**
  - 我们从未真正学习过 CSS 动画，但现在可以简单地说我们想要一个“多汁的 CSS 动画红白糖果条纹加载指示器”，并且它就能做到。
  - 对于 Chrome 扩展清单 v3 也是如此 - 文档一团糟，但幸运的是我们现在不必阅读它们就能完成基本的工作
  - Anthropic 文档（非常糟糕）缺少关于它们返回签名的指导。所以只需用 `curl` 命令试一下，然后把结果放进提示里就行了，哈哈。
- **Modal 就是你所需要的** - 我们选择 Modal 解决了以下四个问题：
  - 解决开发和生产中的 Python 依赖地狱
  - 可并行的代码生成
  - 从本地开发到云托管端点的简单升级路径（未来）
  - 具有重试/退避功能的容错 OpenAI API 调用以及附加存储（未来使用）

> 请订阅 https://latent.space/ 获取更完整的文章以及洞察和反思

### 注意事项

我们正在开发一个 Chrome 扩展，它需要生成图像，因此我们在其中添加了一些特定于用例的代码以跳过销毁/重新生成它们，但我们尚未决定如何概括。

我们无法访问 GPT-4 32k，但如果我们能够，我们会探索将整个 API/SDK 文档转储到上下文中。

现在反馈循环非常慢（使用 GPT-4 生成程序需要大约 2-4 分钟，即使通过 Modal 并行化也是如此（偶尔会更长）），但可以肯定的是，这个时间会随着时间的推移而减少（也请参阅下面的“未来方向”）。

## 未来方向

我们愿意尝试/接受开放问题讨论和 PR 的事项：

- **为每个生成的文件指定 .md 文件**，并提供进一步的提示，这些提示可以在每个文件中微调输出
  - 基本上就像 `popup.html.md` 和 `content_script.js.md` 等
- **为现有代码库引导 `prompt.md`** - 编写一个脚本来读取代码库并写出描述性的、带项目符号的提示，可以生成它
  - 已由 smol pm 完成，但效果还不够好 - 我们希望进行一些集中的打磨和努力，直到我们有一个可以自我生成的 smol 开发者
- **能够安装自己的依赖**
  - 这涉及到执行环境，我们都知道这是导致依赖混乱的路径。如何避免？使用 Docker？使用 Nix？或者 [Web 容器](https://twitter.com/litbid/status/1658154530385670150)？
  - Modal 提供了一个有趣的可能性：生成可以使用 Modal 的函数，这也解决了依赖问题 https://twitter.com/akshat_b/status/1658146096902811657
- **通过运行代码本身进行自我修复，并使用错误作为重新提示的信息**
  - 然而，从 Chrome 扩展环境中获取错误有点困难，所以我们没有尝试这个
- **使用 Anthropic 作为编码层**
  - 你可以运行 `modal run anthropic.py --prompt prompt.md --outputdir=anthropic` 来尝试它

  - 但这并不奏效，因为Anthropic并不擅长按照指示生成文件代码。
- **让代理自动在循环中运行这段代码/监视提示文件**，并在每次更改后，在新的git分支上重新生成代码
  - 代码可以在5个并行的git分支上生成，检查它们的输出只需切换git分支即可