
# smol开发者

<a href="https://app.e2b.dev/agent/smol-developer" target="_blank" rel="noopener noreferrer">
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://app.e2b.dev/api/badge_light">
  <img alt="在e2b上部署代理的按钮" src="https://app.e2b.dev/api/badge"/>
</picture>
</a>


***以人为本且连贯的整体程序合成***，也就是您自己的个人初级开发者

> [构建构建事物的事物！](https://twitter.com/swyx/status/1657578738345979905) 为每个开发者在每种情况下提供一个`smol dev`

这是一个“初级开发者”代理（又名`smol dev`）的原型，它可以为您在提供产品规格后搭建整个代码库，但不会终结世界或过度承诺AGI。与创建和维护特定、僵化、一次性的启动器（如`create-react-app`或`create-nextjs-app`）不同，这基本上是[`create-anything-app`](https://news.ycombinator.com/item?id=35942352)，您可以在其中与您的smol dev紧密循环地开发脚手架提示。

有用、无害且诚实的AI，辅以简单、安全且smol的代码库 - 不到200行的Python和Prompts，因此易于理解和定制。

*不是无代码，不是低代码，而是第三种选择。*

也许是编程的更高阶演变，您仍然需要具备技术能力，但不再需要实现每个细节，至少在搭建框架时不必如此。

## 示例/提示库

- [6分钟视频演示](https://youtu.be/UCo7YeTy-aE) -（抱歉，音频加速了，我们在为Twitter优化，选择不当）
  - 这是最初的smol开发者演示 - 从提示到完整的chrome扩展，请求并存储apikey，生成弹出窗口，读取和传输页面内容，并使用Anthropic Claude有用地总结任何网站，根据输入的长度将模型切换到100k版本
  - 提示位于[prompt.md](https://github.com/smol-ai/developer/blob/main/prompt.md)中，它输出[/exampleChromeExtension](https://github.com/smol-ai/developer/tree/main/examples/exampleChromeExtension)
- `smol-plugin` - 提示转换为ChatGPT插件（[tweet](https://twitter.com/ultrasoundchad/status/1659366507409985536?s=20)，[fork](https://github.com/gmchad/smol-plugin)）

  <img src="https://github.com/smol-ai/developer/assets/6764957/6ffaac3b-5d90-460a-a590-c8a8c004bd36" height=200 />


- [提示转换为Pokemon应用](https://twitter.com/RobertCaracaus/status/1659312419485761536?s=20)

  <img src="https://github.com/smol-ai/developer/assets/6764957/15fa189a-3f52-4618-ac8e-2a77b6500264" height=200 />


- [政治竞选CRM程序示例](https://github.com/smol-ai/developer/pull/22/files)
- [使用GPT-4创建VSCode扩展的经验教训](https://bit.kevinslin.com/p/leveraging-gpt-4-to-automate-the)（也在[HN](https://news.ycombinator.com/item?id=36071342)上）
- [7分钟视频：Smol AI Developer - 用单一提示构建整个代码库](https://www.youtube.com/watch?v=DzRoYc2UGKI) 从提示生成完整的工作OpenAI CLI Python应用程序

  <img src="https://github.com/smol-ai/developer/assets/6764957/e80058f1-ea9c-42dd-87ff-004b61f08f2e" height=200 />


- [12分钟视频：SMOL AI - 用单击AGI开发大型应用程序](https://www.youtube.com/watch?v=zsxyqz6SYp8) 在40分钟内搭建一个令人惊讶的复杂React/Node/MongoDB全栈应用，花费9美元

  <img src="https://github.com/smol-ai/developer/assets/6764957/c51f9f8c-021d-446a-b44d-7a6f48e64550" height=200 />


我正在积极寻找更多示例，请提交您的示例！

抱歉示例不足，我知道这很令人沮丧，但我还没准备好迎接这么多人，哈哈

## 主要分支/替代品

请提交替代实现，并在替代技术栈上部署策略！

- **JS/TS**: https://github.com/PicoCreator/smol-dev-js 一个smol-dev的纯JS变体，允许通过提示进行更小的增量更改（如果您不想做整个spec2code事情），允许您将其实时插入任何项目中（无论好坏）
- **C#/Dotnet**: https://github.com/colhountech/smol-ai-dotnet 用C#实现的版本！
- **Golang**: https://github.com/tmc/smol-dev-go 用Go实现的版本
- https://github.com/gmchad/smol-plugin 通过在smol-developer风格的markdown中指定您的API，自动生成@openai插件
- 您的分支在这里！

## 架构图

像[我们为babyagi所做的](https://twitter.com/swyx/status/1648724820316786688)那样，自然地使用gpt4生成
![image](https://github.com/smol-ai/developer/assets/6764957/f8fc68f4-77f6-43ee-852f-a35fb195430a)

### 创新和洞察

> 请订阅 https://latent.space/ 以获取更完整的文章以及洞察和反思

- **Markdown就是您所需要的** - Markdown是提示整个程序合成的完美方式，因为它很容易混合英语和代码（无论是`variable_names`还是完整的\`\`\`代码块示例）
  - 事实证明，您可以在代码中的提示中指定提示，gpt4会严格遵守
- **复制和粘贴编程**
  - 通过粘贴`curl`输入和输出来教程序如何围绕新API编码（Anthropic的API是在GPT3的知识截止之后）
  - 将错误消息粘贴到提示中并含糊地告诉程序您希望它如何处理。这有点像“日志驱动编程”。
- **通过`cat`整个代码库与错误消息并获取具体修复建议**来调试 - 特别令人愉快！
- **整个程序一致性的技巧** - 我们选择的示例用例，Chrome扩展，在文件之间有很多间接依赖。任何交叉依赖的幻觉都会导致整个程序出错。
  - 我们通过添加一个中间步骤解决了这个问题，要求GPT考虑`shared_dependencies.md`，然后坚持在生成每个文件时使用它。这基本上意味着GPT能够与自己对话......
  - ...但它还不完美。有时`shared_dependencies.md`无法全面理解文件之间的硬依赖。所以我们通过在提示中指定一个特定`name`来解决它。一开始感觉很脏，但它确实有效，而且实际上是一种清晰明确的沟通。
  - 请参阅`prompt.md`了解SOTA smol-dev提示
- **对不熟悉的API的低启动能量**
  - 我们从未真正学习过CSS动画，但现在可以简单地说我们想要一个“多汁的CSS动画红白糖果条纹加载指示器”，它就会做到。
  - 对于Chrome扩展清单v3也是如此 - 文档一团糟，但幸运的是我们现在不必阅读它们就能完成基本的事情
  - Anthropic文档（非常糟糕）缺少关于它们的返回签名的指导。所以只需使用`curl`命令并将结果放入提示中即可。
- **Modal就是您所需要的** - 我们选择Modal来解决4件事：
  - 在开发和生产中解决Python依赖地狱
  - 可并行的代码生成
  - 从本地开发到云托管端点的简单升级路径（未来）
  - 具有重试/退避功能的容错OpenAI API调用以及附加存储（未来使用）

> 请订阅 https://latent.space/ 以获取更完整的文章以及洞察和反思

### 注意事项

我们正在开发一个Chrome扩展，它需要生成图像，因此我们在其中添加了一些特定于用例的代码以跳过销毁/重新生成它们，但我们尚未决定如何概括。

我们无法访问GPT4-32k，但如果我们能够，我们会探索将整个API/SDK文档转储到上下文中。

现在反馈循环非常慢（`time`显示使用GPT4生成程序大约需要2-4分钟，即使有Modal的并行化也会偶尔更高），但可以肯定的是，它会随着时间的推移而减少（也请参阅下面的“未来方向”）。

## 安装

基本上是：

- `git clone https://github.com/smol-ai/developer`。
- 将`.example.env`复制到`.env`并填写您的API密钥。

由于使用Modal作为[自我配置运行时](https://www.google.com/search?q=self+provisioning+runtime)，因此没有需要处理的Python依赖项。

不幸的是，这个项目还使用了其他3个东西：

- Modal.com - [注册](https://modal.com/signup)，然后`pip install modal-client && modal token new`
  - 您可以按照以下说明在不使用Modal的情况下运行此项目：

# smol 开发者

<a href="https://app.e2b.dev/agent/smol-developer" target="_blank" rel="noopener noreferrer">
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://app.e2b.dev/api/badge_light">
  <img alt="在 e2b 上部署代理按钮" src="https://app.e2b.dev/api/badge"/>
</picture>
</a>


***以人为本 & 一致的整体程序综合***，也就是你自己的初级开发者

> [构建构建工具！](https://twitter.com/swyx/status/1657578738345979905) 为每个开发者提供一个 `smol dev`，适用于各种情况

这是一个“初级开发者”代理（也称为 `smol dev`）的原型，它可以根据你提供的产品规格为你构建整个代码库，但不会结束世界或过度承诺 AGI。与创建和维护特定、固定、一次性的启动器（如 `create-react-app` 或 `create-nextjs-app`）不同，这基本上是 [`create-anything-app`](https://news.ycombinator.com/item?id=35942352)，你可以与你的 smol dev 一起紧密循环开发你的脚手架提示。

AI 的有用性、无害性和诚实性与代码库的简单性、安全性和精简性相辅相成 - Python 和 Prompts 不超过 200 行，因此易于理解和定制。

*不是无代码，不是低代码，而是某种第三种东西。*

也许是编程的更高级演变，你仍然需要具备技术能力，但不再需要实现每个细节来构建事物。

## 示例/提示库

- [6 分钟视频演示](https://youtu.be/UCo7YeTy-aE) - （对不起，音频加速了，我们在优化推特，犯了个错误）
  - 这是最初的 smol 开发者演示 - 从提示到完整的 Chrome 扩展，该扩展请求和存储 apikey，生成弹出窗口，读取和传输页面内容，并使用 Anthropic Claude 对任何网站进行有用的摘要，根据输入的长度切换模型，最高可达 100k
  - 提示位于 [prompt.md](https://github.com/smol-ai/developer/blob/main/prompt.md)，输出位于 [/exampleChromeExtension](https://github.com/smol-ai/developer/tree/main/examples/exampleChromeExtension)
- `smol-plugin` - 提示到 ChatGPT 插件（[推文](https://twitter.com/ultrasoundchad/status/1659366507409985536?s=20)，[分支](https://github.com/gmchad/smol-plugin)）

  <img src="https://github.com/smol-ai/developer/assets/6764957/6ffaac3b-5d90-460a-a590-c8a8c004bd36" height=200 />


- [提示到 Pokemon 应用](https://twitter.com/RobertCaracaus/status/1659312419485761536?s=20)

  <img src="https://github.com/smol-ai/developer/assets/6764957/15fa189a-3f52-4618-ac8e-2a77b6500264" height=200 />


- [政治竞选 CRM 程序示例](https://github.com/smol-ai/developer/pull/22/files)
- [创建一个带有 GPT-4 的 VSCode 扩展的经验教训](https://bit.kevinslin.com/p/leveraging-gpt-4-to-automate-the)（也在 [HN](https://news.ycombinator.com/item?id=36071342) 上）
- [7 分钟视频：Smol AI 开发者 - 用一个提示构建整个代码库](https://www.youtube.com/watch?v=DzRoYc2UGKI) 从提示生成一个完整的工作的 OpenAI CLI Python 应用程序

  <img src="https://github.com/smol-ai/developer/assets/6764957/e80058f1-ea9c-42dd-87ff-004b61f08f2e" height=200 />


- [12 分钟视频：SMOL AI - 一键开发大规模应用程序](https://www.youtube.com/watch?v=zsxyqz6SYp8) 在 40 分钟和 9 美元内构建一个令人惊讶的复杂的 React/Node/MongoDB 全栈应用程序

  <img src="https://github.com/smol-ai/developer/assets/6764957/c51f9f8c-021d-446a-b44d-7a6f48e64550" height=200 />


我正在积极寻找更多示例，请提交您的示例！

对于缺乏示例的问题，我很抱歉，我知道这很令人沮丧，但我没有准备好这么多的你们，哈哈

## 主要分支/替代方案

请提供替代实现和在其他堆栈上部署策略！

- **JS/TS**: https://github.com/PicoCreator/smol-dev-js smol-dev 的纯 JS 变体，允许通过提示进行更小的增量更改（如果您不想做整个 spec2code 的事情），允许您将其实时插入任何项目中（好坏参半）
- **C#/Dotnet**: https://github.com/colhountech/smol-ai-dotnet 用 C# 编写！
- **Golang**: https://github.com/tmc/smol-dev-go 用 Go 编写
- https://github.com/gmchad/smol-plugin 通过在 smol-developer 风格的 markdown 中指定 API 来自动生成 @openai 插件
- 在这里放上你的分支！

## 架构图

使用 gpt4 自然生成，就像[我们为 babyagi 所做的那样](https://twitter.com/swyx/status/1648724820316786688)
![image](https://github.com/smol-ai/developer/assets/6764957/f8fc68f4-77f6-43ee-852f-a35fb195430a)

### 创新和见解

> 请订阅 https://latent.space/ 以获取更完整的撰写和见解

- **Markdown 就是你所需要的** - Markdown 是提示整个程序综合的完美方式，因为它很容易混合英语和代码（无论是 `variable_names` 还是整个 \`\`\` 代码块）
  - 结果证明，您可以在提示中的代码中指定提示，并且 gpt4 会完全遵守
- **复制粘贴编程**
  - 通过只粘贴 `curl` 输入和输出来教程序如何在新的 API 周围编码（Anthropic 的 API 在 GPT3 的知识截止日期之后）。 
  - 将错误消息粘贴到提示中，并模糊地告诉程序您希望如何处理它。这有点像“日志驱动编程”。
- **通过 `cat` 整个代码库与错误消息一起调试**，并获得具体的修复建议 - 特别令人愉快！
- **整体程序一致性的技巧** - 我们选择的示例用例，Chrome 扩展，文件之间有许多间接依赖关系。任何交叉依赖的幻觉都会导致整个程序出错。
  - 我们通过添加一个中间步骤来解决这个问题，要求 GPT 在生成每个文件时思考 `shared_dependencies.md`，并坚持在生成每个文件时使用它。这基本上意味着 GPT 能够与自己交流...
  - ... 但它还不完美。`shared_dependencies.md` 有时在理解文件之间的硬依赖关系方面不够全面。所以我们通过在提示中指定一个特定的 `name` 来解决这个问题。一开始感觉有点不好，但它确实有效，最终这只是明确明确的沟通。
  - 有关 SOTA smol-dev 提示，请参阅 `prompt.md`
- **对于不熟悉的 API 的低激活能量**
  - 我们从未真正学习过 css 动画，但现在可以说我们想要一个“多汁的 CSS 动画红白条纹加载指示器”，然后它就能做到。
  - Chrome 扩展清单 v3 也是如此 - 文档是一团糟，但幸运的是我们现在不必阅读它们就可以完成基本的事情
  - Anthropic 的文档（很糟糕）缺少有关返回签名的指导。所以只需将其 curl 并将其转储到提示中，哈哈。
- **模态是你所需要的** - 我们选择了 Modal 来解决 4 个问题：
  - 解决开发和生产中的 Python 依赖关系地狱
  - 可并行的代码生成
  - 从本地开发到云托管端点的简单升级路径（将来）
  - 具有重试/退避和附加存储（供将来使用）的容错 openai api 调用

> 请订阅 https://latent.space/ 以获取更完整的撰写和见解

### 注意事项

我们正在开发一个 Chrome 扩展，它需要生成图像，因此我们在其中添加了一些特定于用例的代码，以跳过销毁/重新生成图像的步骤，我们尚未决定如何将其泛化。

我们没有访问 GPT4-32k，但如果有的话，我们将尝试将整个 API/SDK 文档转储到上下文中。

反馈循环现在非常缓慢（`time` 表明使用 GPT4 生成程序大约需要 2-4 分钟，即使使用并行化也会偶尔达到更高水平），但可以肯定的是，随着时间的推移，这个时间会减少（请参阅下面的“未来发展方向”）。

## 安装

基本上是：

- `git clone https://github.com/smol-ai/developer`.
- 将 `.example.env` 复制到 `.env`，并填写您的 API 密钥。

由于使用 Modal 作为[自我配置运行时](https://www.google.com/search?q=self+provisioning+runtime)，因此没有 Python 依赖关系需要处理。

不幸的是，该项目还使用了其他 3 个东西：

- Modal.com - [注册](https://modal.com/signup)，然后 `pip install modal-client && modal token new`
  - 您可以按照以下说明在没有 Modal 的情况下运行此项目：
  - `pip install -r requirements.txt`
  - `export OPENAI_API_KEY=sk-xxxxxx`（您的 openai api 密钥在此处）
  - `python main_no_modal.py YOUR_PROMPT_HERE`
- GPT-4 api（私人测试版）- 该项目现在默认使用 `gpt-3.5-turbo`，但显然不会那么好。我们正在开发托管版本，因此您可以在我们的密钥上尝试一下。
- （仅适用于演示项目）anthropic claude 100k context api（私人测试版）- 并不重要，除非您确实想重现我的演示

您将不得不根据自己的基础设施在分支上调整此代码。请打开问题/PR，我很乐意在这里突出显示您的分叉。

### 尝试演示视频中的示例 chrome 扩展

/examples/exampleChromeExtension 文件夹包含一个 Chrome Manifest V3 扩展，它读取当前页面，并提供一个弹出 UI，其中包含页面标题+内容和提示文本区域（使用我们指定的默认值）。当用户点击提交时，它会将页面标题+内容发送到 Anthropic Claude API 以及最新的提示以进行总结。用户可以修改该提示并重新发送提示+内容以获得内容的另一个摘要视图。

- 转到 Chrome 中的管理扩展程序
- 装载已拆封的
- 在文件系统中找到相关文件夹并加载它
- 去任何内容丰富的网站
- 点击可爱的小鸟
- 看到它起作用并感到高兴

整个扩展是由 `prompt.md` 中的提示生成的（图像除外），并且是通过在迭代过程中向提示添加更多单词来逐渐构建的。

## 用法：smol 开发

基本用法（默认情况下它使用 `gpt-3.5-turbo` 运行，但如果您有访问权限，我们强烈建议使用 `gpt-4` 运行）

```bash
# 内联提示
modal run main.py --prompt "a Chrome extension that, when clicked, opens a small window with a page where you can enter a prompt for reading the currently open page and generating some response from openai" --model=gpt-4
```

一段时间后，您可以将提示提取到文件中，只要您的“prompt”以 .md 扩展名结尾，我们就会去查找该文件

```bash
# 在 markdown 文件中的提示
modal run main.py --prompt prompt.md --model=gpt-4
```

每次运行此命令时，生成的目录都会被删除（图像除外），并且所有文件都会从头开始重写。

在`shared_dependencies.md`文件中是一个帮助程序文件，可确保文件之间的一致性。这正在扩展为官方的`--plan`功能（请参阅[https://github.com/smol-ai/developer/issues/12](https://github.com/smol-ai/developer/issues/12)）

### smol 开发的单文件模式

如果您对提示进行了调整，并且只希望它影响一个文件，并保留其余文件，请指定文件参数：

```bash
modal run main.py --prompt prompt.md  --file popup.js
```

### 没有 modal.com 的 smol 开发

默认情况下，main.py 使用 Modal，因为它提供了一个很好的托管体验升级路径（即将推出，因此您可以在不需要 GPT4 密钥访问的情况下尝试它）。

但是，如果您只想在自己的计算机上运行它，则可以按照以下说明运行 smol dev w/o Modal：

```bash
pip install -r requirements.txt
export OPENAI_API_KEY=sk-xxxxxx # your openai api key here)

python main_no_modal.py YOUR_PROMPT_HERE
```

如果没有给出命令行参数，**并且**文件`prompt.md`存在，则main函数将自动使用`prompt.md`文件。所有其他命令行参数均保留为默认值。*这对于那些在Windows版PyCharm的`venv`设置上使用“运行”函数的人来说非常方便，因为没有机会输入命令行参数。*谢谢[@danmenzies](https://github.com/smol-ai/developer/pull/55)

## 用法：smol调试器

*这是一个测试版功能，非常非常 MVP，只是一个概念验证*

在上下文中获取生成目录的全部内容，输入错误，获取响应。这基本上利用了更长（32k-100k）的上下文，因此我们基本上不必对源代码进行任何嵌入。

```bash
modal run debugger.py --prompt "Uncaught (in promise) TypeError: Cannot destructure property 'pageTitle' of '(intermediate value)' as it is undefined.    at init (popup.js:59:11)"

# gpt4
modal run debugger.py --prompt "your_error msg_here" --model=gpt-4
```

## 用法：smol pm

*这比 beta 还要糟糕，这是一种“让我们看看会发生什么”的实验*

在上下文中获取生成目录的全部内容，并获得可以综合整个程序的提示。基本上是 smol dev，相反。

```bash
modal run code2prompt.py # ~0.5 second with gpt 3.5

# use gpt4
modal run code2prompt.py --model=gpt-4 # 2 mins, MUCH better results
```

我们已经对两者进行了指示性运行，存储在 Examples/code2prompt/code2prompt-gpt3.md 和 Examples/code2prompt/code2prompt-gpt4.md 中。请注意，gpt4 在快速设计其未来自我方面有多么出色。

当然，我们必须尝试 code2prompt2code...

```bash
# add prompt... this needed a few iterations to get right
modal run code2prompt.py --prompt "make sure all the id's of the DOM elements, and the data structure of the page content (stored with {pageTitle, pageContent }) , referenced/shared by the js files match up exactly. take note to only use Chrome Manifest V3 apis. rename the extension to code2prompt2code" --model=gpt-4 # takes 4 mins. produces semi working chrome extension copy based purely on the model-generated description of a different codebase

# must go deeper
modal run main.py --prompt code2prompt-gpt4.md --directory code2prompt2code
```

我们将代码库的多层生成油炸的社会和技术影响作为练习留给读者。

## 使用开发容器进行开发

> 这是[new addition](https://github.com/smol-ai/developer/pull/30)！请尝试一下，如果有任何问题，请发送修复程序。

我们为本项目配置了一个开发容器，它提供了一个隔离且一致的开发环境。此方法非常适合使用 Visual Studio Code 的远程容器扩展或 GitHub 的 Codespaces 的开发人员。

如果您的计算机上安装了 [VS Code](https://code.visualstudio.com/download) 和 [Docker](https://www.swyx.io/running-docker-without-docker-desktop)，则可以利用 devcontainer 创建一个隔离环境，其中会自动安装和配置所有依赖项。这是确保在不同机器上获得一致开发体验的好方法。

以下是使用 devcontainer 的步骤：

1. 在 VS Code 中打开此项目。
2. 当提示“在容器中重新打开”时，选择“在容器中重新打开”。这将开始构建由 .devcontainer 目录中的 Dockerfile 和 .devcontainer.json 定义的 devcontainer 的过程。
3. 等待构建完成。第一次会有点长，因为它会下载并构建所有内容。未来的负载将会快得多。
4. 构建完成后，VS Code 窗口将重新加载，您现在可以在 devcontainer 中工作。

<details>
<summary> 开发容器的好处 </summary>


1. **一致的环境**：每个开发人员都在相同的开发设置中工作，消除了“它可以在我的机器上运行”问题并简化新贡献者的加入。

2. **沙箱**：您的开发环境与本地计算机隔离，允许您处理具有不同依赖关系的多个项目而不会发生冲突。

3. **环境的版本控制**：就像对源代码进行版本控制一样，您可以对开发环境进行相同的操作。如果依赖项更新引入问题，可以轻松恢复到之前的状态。

4. **更轻松的 CI/CD 集成**：如果您的 CI/CD 管道使用 Docker，您的测试环境将与本地开发环境相同，从而确保一致性 across development, testing, and production setups。

5. **可移植性**：此设置可以在任何安装了 Docker 和适当 IDE 的计算机上使用。只需克隆存储库并启动容器即可。
</details>


## 未来发展方向

尝试/愿意接受开放问题讨论和 PR 的事情：

- **为每个生成的文件指定 .md 文件**，并提供可以在每个文件中微调输出的进一步提示
  - 所以基本上就像 popup.html.md 和 content_script.js.md 等等
- **引导**现有代码库的`提示.md` - 编写一个脚本来读取代码库并编写描述性、项目符号提示
  - 由 smol pm 完成，但还不是很好 - 希望进行一些集中的打磨/努力，直到我们有 quine smol 开发人员可以生成自己 lmao
- **能够安装自己的依赖项**
  - 这取决于执行环境，我们都知道这是导致依赖疯狂的路径。如何避免？_dockerize?_ _nix?_ [网络容器](https://twitter.com/litbid/status/1658154530385670150)？
  - Modal有一个有趣的可能性：生成使用模态的函数，这也解决了依赖问题 https://twitter.com/akshat_b/status/1658146096902811657
- **通过运行代码本身进行自我修复，并使用错误作为重新提示的信息**
  - 然而，从 chrome 扩展环境中获取错误有点困难，所以我们没有尝试这个
- **使用anthropic作为编码层**
  - 你可以运行 modal run anthropic.py --prompt提示.md --outputdir=anthropic 来尝试一下
  - 但它不起作用，因为Anthropic不太擅长按照指示生成文件代码。
- **制作能够自动在循环中运行此代码/监控提示文件的代理**，并在每次更改后，在新的git分支上重新生成代码
  - 代码可以在5个并行的git分支上生成，检查它们的输出只需切换git分支即可