
一个 Chrome Manifest V3 扩展，它读取当前页面，并提供一个弹出 UI，其中包含页面标题+内容和用于提示的文本区域（使用我们指定的默认值）。当用户点击提交时，它会将页面标题+内容连同最新的提示发送到 Anthropic Claude API 以进行总结。用户可以修改该提示并重新发送提示+内容以获得内容的另一个摘要视图。

- 仅当单击时：
  - 它在当前打开的标签页上注入内容脚本 `content_script.js`，并访问当前打开页面的标题 `pageTitle` 和主要内容（innerText）`pageContent`（通过注入的内容脚本提取，并使用 `storePageContent` 操作发送）
  - 在后台，接收 `storePageContent` 数据并存储它
  - 仅当新页面内容被存储后，才会弹出一个全高度窗口，并带有简约风格的 html 弹出窗口
  - 在弹出脚本中
    - 弹出窗口应显示一个 10px 高的圆角 css 动画红白相间的糖果条纹加载指示器 `loadingIndicator`，同时等待 anthropic api 返回
      - 中心显示当前正在获取的页面标题和一个运行计时器，显示自调用开始以来经过的时间
      - 在 api 调用开始之前不显示它，并在调用结束时隐藏它。
    - 使用 `getPageContent` 操作检索页面内容数据（后台监听 `getPageContent` 操作并检索该数据）并在弹出窗口顶部显示标题
    - 检查扩展存储中是否有 `apiKey`，如果未存储，则请求一个 Anthropic Claude API 密钥并存储它。
    - 在弹出窗口的底部，显示一个垂直可调整大小的表单，其中包含：
      - 一个 2 行文本区域，其 id 和标签为 `userPrompt`
        - `userPrompt` 的默认值为
          ```js
          defaultPrompt = `Please provide a detailed, easy to read HTML summary of the given content.`;
          ```
      - 一个 4 行文本区域，其 id 和标签为 `stylePrompt`
        - `stylePrompt` 的默认值为
          ```js
          defaultStyle = `Respond with 3-4 highlights per section with important keywords, people, numbers, and facts bolded in this HTML format:
          
          <h1>{title here}</h1>
          <h3>{section title here}</h3>
          <details>
            <summary>{summary of the section with <strong>important keywords, people, numbers, and facts bolded</strong> and key quotes repeated}</summary>
            <ul>
              <li><strong>{first point}</strong>: {short explanation with <strong>important keywords, people, numbers, and facts bolded</strong>}</li>
              <li><strong>{second point}</strong>: {same as above}</li>
              <li><strong>{third point}</strong>: {same as above}</li>
              <!-- a fourth point if warranted -->
            </ul>
          </details>
          <h3>{second section here}</h3>
          <p>{summary of the section with <strong>important keywords, people, numbers, and facts bolded</strong> and key quotes repeated}</p>
          <details>
            <summary>{summary of the section with <strong>important keywords, people, numbers, and facts bolded</strong> and key quotes repeated}</summary>
            <ul>
              <!-- as many points as warranted in the same format as above -->
            </ul>
          </details>
          <h3>{third section here}</h3>
          <!-- and so on, as many sections and details/summary subpoints as warranted -->
          
          With all the words in brackets replaced by the summary of the content. Sanitize non-visual HTML tags with HTML entities, so <template> becomes &lt;template&gt; but <strong> stays the same. Only draw from the source content, do not hallucinate. Finally, end with other questions that the user might want answered based on this source content:
          
          <hr>
          <h2>Next prompts</h2>
          <ul>
            <li>{question 1}</li>
            <li>{question 2}</li>
            <li>{question 3}</li>
          </ul>`;
          ```
      - 在最后一行的两侧，
        - 以及一个样式精美的提交按钮，其 id 为 `sendButton`（点击时具有“按下”效果的触觉样式）
      - 仅当点击 `sendButton` 时，才调用 Anthropic 模型端点 https://api.anthropic.com/v1/complete：
        - 附加页面标题
        - 追加页面内容
        - 添加由以下内容串联而成的提示
          ```js
          finalPrompt = `Human: ${userPrompt} \n\n ${stylePrompt} \n\n Assistant:`;
          ```
        - 并使用 `claude-instant-v1` 模型（如果 `pageContent` 少于 70k 字）或 `claude-instant-v1-100k` 模型（如果更多）
        - 请求最大令牌数 = 页面内容长度的 25% 或 750 个词中的较高者
        - 如果在上一个 api 调用仍在进行时又发生了另一个提交事件，请取消该事件并启动新的事件
    - 在弹出窗口顶部的 div 中呈现 Anthropic 生成的结果，该 div 的 id 为 `content`

重要细节：

- 它必须在浏览器环境中运行，因此不允许使用 Node.js API。

- anthropic API 的返回签名是
curl https://api.anthropic.com/v1/complete\
-H "x-api-key: $API_KEY"\
-H 'content-type: application/json'\
-d '{
"prompt": "\n\nHuman: 告诉我一首关于树的俳句\n\nAssistant: ",
"model": "claude-v1", "max_tokens_to_sample": 1000, "stop_sequences": ["\n\nHuman:"]
}'
{"completion":"这是一首关于树木的俳句：\n\n沉默的哨兵，\n在林中庄严地矗立，\n枝条伸向天空。","stop":"\n\nHuman:","stop_reason":"stop_sequence","truncated":false,"log_id":"f5d95cf326a4ac39ee36a35f434a59d5","model":"claude-v1","exception":null}

- 在发送到 Anthropic 的字符串提示中，首先包含页面标题和页面内容，然后附加提示，并以空间明确垂直分隔。

- 如果 Anthropic API 调用返回 401 错误，处理该问题的方法是清除存储的 anthropic API 密钥并再次请求它。

- 添加样式以确保弹出窗口的样式遵循网页设计的基本规则，例如在主体周围有边距和使用系统字体栈。

- 使用 `<link rel="stylesheet" href="https://unpkg.com/mvp.css@1.12/mvp.css">` 设置弹出主体的样式，但要求主体边距为 16，最小宽度为 400，高度为 600。

## 调试笔记

在 background.js 内部，直接取得 getPageContent 响应

```js
chrome.runtime.onMessage.addListener((request, sender, sendResponse) => {
  if (request.action === 'storePageContent') {
    // 不访问 request.pageContent
    chrome.storage.local.set({ pageContent: request }, () => {
      sendResponse({ success: true });
    });
  } else if (request.action === 'getPageContent') {
    chrome.storage.local.get(['pageContent'], (result) => {
      // 不访问 request.pageContent
      sendResponse(result);
    });
  }
  return true;
});
```

在 popup.js 内部，更新对 `requestAnthropicSummary` 函数的调用，以在 `popup.js` 中传递 `apiKey`：

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

在 popup.js 中，将 defaultPrompt 存储在顶层。同时，为 anthropic 提示提供 HTML 格式
