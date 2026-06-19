---
"mattpocock-skills": patch
---

扩展 **`triage`** 技能，使其能够 triage 外部拉取请求，把一个 PR 当作附带代码的 issue，让它走相同的角色与状态机。PR 与 issue 并排内联流转（由每个仓库的 setup 开关控制），发现阶段只暴露外部 PR，仅针对 bug 的 "reproduce" 步骤被泛化为单一的 "verify the claim" 步骤，并通过一次冗余检查把已经实现的请求解析为 `wontfix`，而不污染超出范围的知识库。`setup-matt-pocock-skills` 新增了「PR 作为请求来源」的开关，适用于 GitHub/GitLab。
