---
name: obsidian-vault
description: 在 Obsidian vault 中搜索、创建和管理笔记，支持 wikilink 与索引笔记。当用户希望查找、创建或整理 Obsidian 中的笔记时使用。
---

# Obsidian Vault

## Vault 位置

`/mnt/d/Obsidian Vault/AI Research/`

大体上在根层级保持扁平。

## 命名约定

- **索引笔记**：聚合相关主题（例如 `Ralph Wiggum Index.md`、`Skills Index.md`、`RAG Index.md`）
- 所有笔记名采用 **Title Case**
- 不用文件夹来组织——改用链接和索引笔记

## 链接

- 使用 Obsidian 的 `[[wikilinks]]` 语法：`[[Note Title]]`
- 笔记在底部链接到依赖项/相关笔记
- 索引笔记就是一组 `[[wikilinks]]`

## 工作流

### 搜索笔记

```bash
# Search by filename
find "/mnt/d/Obsidian Vault/AI Research/" -name "*.md" | grep -i "keyword"

# Search by content
grep -rl "keyword" "/mnt/d/Obsidian Vault/AI Research/" --include="*.md"
```

或者直接对 vault 路径使用 Grep/Glob 工具。

### 创建新笔记

1. 文件名使用 **Title Case**
2. 将内容写成一个学习单元（遵循 vault 规则）
3. 在底部添加指向相关笔记的 `[[wikilinks]]`
4. 如果属于某个编号序列，采用分层编号方案

### 查找相关笔记

在整个 vault 中搜索 `[[Note Title]]` 以查找反向链接：

```bash
grep -rl "\\[\\[Note Title\\]\\]" "/mnt/d/Obsidian Vault/AI Research/"
```

### 查找索引笔记

```bash
find "/mnt/d/Obsidian Vault/AI Research/" -name "*Index*"
```
