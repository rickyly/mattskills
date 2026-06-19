---
name: handoff
description: 把当前对话压缩成一份交接文档，供另一个代理接手。
argument-hint: "What will the next session be used for?"
disable-model-invocation: true
---

写一份交接文档，概括当前对话，让一个全新的代理能够继续工作。保存到用户操作系统的临时目录，而不是当前工作区。

在文档中加入一节 "suggested skills"，列出该代理应当调用的技能。

不要重复其他工件（PRD、计划、ADR、issue、提交、diff）中已经记录的内容。改用路径或 URL 引用它们。

对任何敏感信息脱敏，例如 API key、密码或个人身份信息。

如果用户传入了参数，把它们当作对下一个会话聚焦内容的描述，并据此调整文档。
