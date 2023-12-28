
一个 Chrome Manifest V3 扩展，它读取当前页面，并提供一个弹出 UI，将页面内容发送到 Anthropic Claude API 以及一个总结提示，并允许用户修改该提示并重新发送提示+内容以获取内容的另一个摘要视图。

- 点击时：
  - 它在当前打开的标签页上注入内容脚本 `content_script.js`，并访问当前打开页面的标题 `pageTitle` 和主要内容 `pageContent`（通过注入的内容脚本提取，并使用 `storePageContent` 动作发送）
  - 在后台，接收 `storePageContent` 数据并存储
  - 弹出一个小窗口，带有简单、现代、光滑、极简风格的 HTML 弹出窗口
  - 在弹出脚本中
    - 使用 `getPageContent` 动作检索页面内容数据（后台监听 `getPageContent` 动作并检索该数据）
    - 检查扩展存储中是否有 `apiKey`，如果没有，则请求一个 Anthropic Claude 的 API 密钥并存储
    - 调用 Anthropic 模型端点 https://api.anthropic.com/v1/complete 使用 `claude-instant-v1-100k` 模型，包括：
      - 添加页面标题
      - 添加页面内容
      - 添加一个提示，要求提供一个详细、易于阅读的 HTML 摘要，每个部分有 3-4 个重点，重要关键词加粗，重要链接保留。格式如下：
        ```js
        defaultPrompt = `Human: Please provide a detailed, easy to read HTML summary of the given content 
        with 3-4 highlights per section with important keywords bolded and important links preserved, in this format:
        
        <h1>{title here}</h1>
        <h2>{section title here}</h2>
        <ul>
          <!-- if there is an important, relevant image --><img src="{main image, if any}" style="height:8rem">
          <li><strong>{first point}</strong>: {short explanation with details, with any relevant links included}</li>
          <li><strong>{second point}</strong>: {short explanation with details, with any relevant links included}</li>
          <li><strong>{third point}</strong>: <!-- etc -->
          <!-- etc -->
        </ul>
        <h2>{second section here}</h2>
        <ul>
          <!-- etc -->
        </ul>
        <!-- etc -->
        
        With all the words in brackets replaced by the summary of the content. Only draw from the source content, do not hallucinate.
        
        Assistant:`;
        ```js
        ```
    - 在弹出窗口的 div 中渲染 Anthropic 生成的 HTML 摘要，div 的 id 为 `content`
  - 在弹出窗口的底部，显示一个带有 id 为 `userPrompt` 的文本区域，内含一个简短的默认提示值，以及一个带有 id 为 `sendButton` 的提交按钮。
    - 当点击 `sendButton` 时，允许用户使用相同的页面标题和页面内容但不同的提示（来自 `userPrompt`）重新向 Anthropic 发起请求。
    - 在等待 Anthropic API 调用完成时禁用这些输入

重要细节：

- 它必须在浏览器环境中运行，因此不允许使用 Node.js API。

- 弹出窗口应显示“读取 {TITLE} 的页面内容”的消息，并在等待 Anthropic API 返回时显示一个大而引人注目的 CSS 动画糖果条纹加载指示器 `loadingIndicator`，其中 TITLE 是页面标题。在 API 调用开始之前不显示它，并在调用结束时清除它。

- Anthropic API 的返回签名是：
```
curl https://api.anthropic.com/v1/complete\
-H "x-api-key: $API_KEY"\
-H 'content-type: application/json'\
-d '{
"prompt": "\n\nHuman: 告诉我一首关于树的俳句\n\nAssistant: ",
"model": "claude-v1", "max_tokens_to_sample": 1000, "stop_sequences": ["\n\nHuman:"]
}'
{"completion":"这是一首关于树的俳句：\n\n沉默的哨兵，\n在树林中庄严地矗立，\n枝条伸向天空。","stop":"\n\nHuman:","stop_reason":"stop_sequence","truncated":false,"log_id":"f5d95cf326a4ac39ee36a35f434a59d5","model":"claude-v1","exception":null}
```

- 在发送到 Anthropic 的字符串提示中，首先包含页面标题和页面内容，然后附加提示，通过空间清晰地垂直分隔。

- 如果 Anthropic API 调用返回 401 错误，处理方法是清除存储的 Anthropic API 密钥并重新请求。

- 添加样式以确保弹出窗口的样式遵循网页设计的基本规则，例如在主体周围设置边距和使用系统字体栈。

- 为弹出窗口的主体设置最小宽度为 400px 和最小高度为 600px。

## 调试笔记

在 background.js 内部，直接取得 getPageContent 响应

```js
chrome.runtime.onMessage.addListener((request, sender, sendResponse) => {
  if (request.action === 'storePageContent') {
    // dont access request.pageContent
    chrome.storage.local.set({ pageContent: request }, () => {
      sendResponse({ success: true });
    });
  } else if (request.action === 'getPageContent') {
    chrome.storage.local.get(['pageContent'], (result) => {
      // dont access request.pageContent
      sendResponse(result);
    });
  }
  return true;
});
```

在 popup.js 内部，更新对 `requestAnthropicSummary` 的函数调用，以在 `popup.js` 中传递 `apiKey`：

```javascript
chrome.storage.local.get(['apiKey'], (result) => {
  const apiKey = result.apiKey;
  requestAnthropicSummary(defaultPrompt, apiKey);
});

sendButton.addEventListener('click', () => {
  chrome.storage.local.get(['apiKey'], (result) => {
    const apiKey = result.apiKey;
    requestAnthropicSummary(userPrompt.value, apiKey);
  });
});
```

在 popup.js 中，将 `defaultPrompt` 存储在顶层，并为 Anthropic 提示提供 HTML 格式。
