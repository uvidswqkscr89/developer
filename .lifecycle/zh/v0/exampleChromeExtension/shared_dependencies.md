
该应用程序是：一个 Chrome Manifest V3 扩展，它读取当前页面，并提供一个弹出 UI，将页面内容连同一个总结提示发送到 Anthropic Claude API，并允许用户修改该提示，然后重新发送提示+内容以获取内容的另一个摘要视图。

我们决定生成的文件包括：content_script.js、background.js、popup.html、popup.js、popup.css

共享依赖项：

1. 导出的变量：
   - pageTitle
   - pageContent

2. 数据模式：
   - storePageContent 操作数据：{ action: 'storePageContent', pageTitle, pageContent }
   - getPageContent 操作数据：{ action: 'getPageContent' }

3. DOM 元素的 ID 名称：
   - content
   - userPrompt
   - sendButton
   - loadingIndicator

4. 消息名称：
   - storePageContent
   - getPageContent

5. 函数名称：
   - requestAnthropicSummary

6. API 端点：
   - https://api.anthropic.com/v1/complete

7. 模型名称：
   - claude-instant-v1-100k

8. 默认提示：
   - defaultPrompt
