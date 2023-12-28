
# 人工智能克劳德摘要扩展

一个 Chrome 扩展，使用 Anthropic Claude API 来汇总网页内容。

## 文件

### popup.js

该文件包含扩展弹出窗口的主要逻辑。它监听 DOMContentLoaded 事件并初始化以下 DOM 元素：

- `userPrompt`：一个文本区域，供用户输入提示。
- `stylePrompt`：一个文本区域，供用户输入风格提示。
- `maxTokens`：一个输入字段，供用户设置摘要的最大令牌数。
- `sendButton`：一个按钮，用于提交表单并请求摘要。
- `content`：一个 div，用于展示摘要。
- `loadingIndicator`：一个 div，在等待摘要时显示加载指示器。

`sendButton` 添加了一个点击事件监听器，用于调用 `requestAnthropicSummary` 函数。此函数向后台脚本发送消息以获取页面内容，结合用户输入和页面内容构建提示，并向 Anthropic Claude API 发送 POST 请求。然后将响应显示在 `content` div 中。

### styles.css

该文件包含弹出窗口的 CSS 样式。它定义了 body、textarea、input、button、content 和 loadingIndicator 元素的样式。

### background.js

该文件包含扩展的后台脚本。它监听以下事件：

- `chrome.action.onClicked`：当点击扩展图标时，在当前标签页执行 content_script.js 文件。
- `chrome.runtime.onMessage`：监听具有以下动作的消息：
  - `storePageContent`：使用标签页 ID 作为键，将页面标题和内容存储在本地存储中。
  - `getPageContent`：使用标签页 ID 从本地存储中检索页面标题和内容，并将其作为响应发送。

### popup.html

该文件包含弹出窗口的 HTML 结构。它包括以下元素：

- 一个表单，内含 userPrompt、stylePrompt 和 maxTokens 的标签和输入框。
- 一个 ID 为 "sendButton" 的提交按钮。
- 一个 ID 为 "content" 的 div，用于展示摘要。
- 一个 ID 为 "loadingIndicator" 的 div，用于显示加载指示器。

该文件还包括指向 styles.css 文件的链接和引入 popup.js 文件的 script 标签。

### shared_dependencies.md

该文件列出了扩展中使用的共享依赖项、变量、DOM 元素 ID、消息名称和函数名称。

### content_script.js

该文件包含一个名为 `storePageContent` 的函数，该函数检索页面标题和内容，并向后台脚本发送包含动作 "storePageContent" 和包含标题和内容的对象数据的消息。

它还监听带有动作 "storePageContent" 的消息，并在接收到时调用 `storePageContent` 函数。

### manifest.json

该文件包含扩展的清单。它包括以下属性：

- `manifest_version`：设置为 2。
- `name`：设置为 "Anthropic Claude 摘要扩展"。
- `version`：设置为 "1.0"。
- `description`：设置为 "一个使用 Anthropic Claude API 来汇总网页内容的 Chrome 扩展"。
- `permissions`：包括 "activeTab" 和 "storage"。
- `action`：定义扩展的 default_popup 和 default_icon。
- `background`：包含 background.js 脚本，并将 "persistent" 属性设置为 false。
- `content_scripts`：包含 content_script.js 文件，并匹配所有 URL。
- `icons`：定义扩展的图标。

**注意**：确保所有 JavaScript 文件中引用/共享的 DOM 元素 ID 和 `pageContent` 数据结构完全匹配。只使用 Chrome Manifest V3 API。将扩展名重命名为 "code2prompt2code"。
