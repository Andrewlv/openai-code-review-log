# OpenAi 代码评审.
### 😀代码评分：85
#### 😀代码逻辑与目的：
该代码是一个基于 GitHub Actions 的自动化代码评审组件，旨在通过 ChatGLM 进行代码审查，并使用微信公众号发送通知。

#### 🤔问题点：
1. **代码结构**：代码中包含了一个图片文件，但没有在 Markdown 文档中引用或描述该图片的作用。
2. **安全性**：配置中直接使用了 API 密钥和敏感信息，但没有提及加密或安全的存储方式。
3. **性能**：代码中使用了 `wget` 命令下载 JAR 文件，这可能会影响性能，特别是当网络条件不佳时。
4. **可维护性**：代码中的一些步骤（如设置 JDK、创建目录）可能会随着项目的变化而变化，但没有提供足够的灵活性。

#### 🎯修改建议：
1. **引用图片**：在 Markdown 文档中添加图片的引用和描述。
2. **安全存储**：使用 GitHub Secrets 的安全方式存储敏感信息，避免直接在代码中暴露。
3. **优化性能**：考虑使用 Git LFS 或其他方式来处理大型文件，以减少网络请求。
4. **提高可维护性**：将配置参数和步骤封装成函数或模块，以便于管理和维护。

#### 💻修改后的代码：
```markdown
# 基于ChatGLM的自动代码评审组件

提交合并分支的代码，则触发代码评审，并写入评审日志文件。完成后发送公众号模板消息通知，点击<详情>查看评审细节。

## 使用方法
基于 GitHub Actions + ChatGLM + Git/GitHub + 公众号模板消息 串联出从代码提交获取通知，Git 检出分支变化，在使用 ChatGLM 进行代码和写入日志，再发送消息通知完成整个链路。

1. 申请ChatGLM
* CHATGLM_APIKEYSECRET： https://open.bigmodel.cn/usercenter/apikeys
* CHATGLM_APIHOST：https://open.bigmodel.cn/api/paas/v4/chat/completions

2. 申请 GitHub 仓库
* 工程库：https://github.com/xfg-studio-project/openai-code-review-test - 创建一个自己的，并提交代码。
* 日志库：https://github.com/xfg-studio-project/openai-code-review-log - 你创建一个自己的。

3. 申请 GitHub Token
地址：https://github.com/settings/tokens
创建后，保存生成的 Token，用于配置到 GitHub Actions 参数中

4. 微信公众号配置
* 申请地址 https://mp.weixin.qq.com/debug/cgi-bin/sandboxinfo?action=showinfo&t=sandbox/index
* 这个测试公众号等同于企业公众号，有对应的模板消息。
* 申请后，你就会获得 appID、appsecret、tourse - 就是谁关注了公众号，就会展示一个分配的微信号，推送模板消息就是给这个用户推送。
* 模板消息，自己新建一个。之后就获得ID。消息格式如下；
* ```
  项目：{{repo_name.DATA}} 分支：{{branch_name.DATA}} 作者：{{commit_author.DATA}} 说明：{{commit_message.DATA}}
  ```

5. GitHub Actions 配置
5.1 配置参数
地址：https://github.com/xfg-studio-project/openai-code-review-test/settings/secrets/actions - 换成你的项目工程，进入到 Setting -> Secrets and variables -> Actions -> Repository secrets -> New repository secret

5.2 配置脚本
[此处省略配置脚本，建议按照上述建议进行优化]

把以上脚本粘贴到你的 GitHub Actions 中，之后保存。
接下来你提交代码就会自动触发代码评审啦。💐 赶紧玩一下吧！看看智能的AI评审能力！
```

#### 代码中的优点：
- 提供了详细的配置步骤和说明。
- 使用了 GitHub Actions 实现自动化流程。
- 代码审查和通知流程清晰。