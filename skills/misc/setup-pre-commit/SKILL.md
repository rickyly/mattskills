---
name: setup-pre-commit
description: 在当前仓库中配置 Husky pre-commit 钩子，集成 lint-staged（Prettier）、类型检查与测试。当用户希望添加 pre-commit 钩子、配置 Husky、配置 lint-staged，或添加提交时的格式化/类型检查/测试时使用。
---

# 配置 Pre-Commit 钩子

## 本技能配置的内容

- **Husky** pre-commit 钩子
- **lint-staged** 对所有暂存文件运行 Prettier
- **Prettier** 配置（若缺失）
- pre-commit 钩子中的 **typecheck** 与 **test** 脚本

## 步骤

### 1. 检测包管理器

检查 `package-lock.json`（npm）、`pnpm-lock.yaml`（pnpm）、`yarn.lock`（yarn）、`bun.lockb`（bun）。哪个存在就用哪个。不确定时默认用 npm。

### 2. 安装依赖

作为 devDependencies 安装：

```
husky lint-staged prettier
```

### 3. 初始化 Husky

```bash
npx husky init
```

这会创建 `.husky/` 目录，并向 package.json 添加 `prepare: "husky"`。

### 4. 创建 `.husky/pre-commit`

写入此文件（Husky v9+ 无需 shebang）：

```
npx lint-staged
npm run typecheck
npm run test
```

**调整**：将 `npm` 替换为检测到的包管理器。如果仓库的 package.json 中没有 `typecheck` 或 `test` 脚本，则省略对应的行并告知用户。

### 5. 创建 `.lintstagedrc`

```json
{
  "*": "prettier --ignore-unknown --write"
}
```

### 6. 创建 `.prettierrc`（若缺失）

仅在不存在 Prettier 配置时才创建。使用以下默认值：

```json
{
  "useTabs": false,
  "tabWidth": 2,
  "printWidth": 80,
  "singleQuote": false,
  "trailingComma": "es5",
  "semi": true,
  "arrowParens": "always"
}
```

### 7. 验证

- [ ] `.husky/pre-commit` 存在且可执行
- [ ] `.lintstagedrc` 存在
- [ ] package.json 中的 `prepare` 脚本为 `"husky"`
- [ ] `prettier` 配置存在
- [ ] 运行 `npx lint-staged` 以验证其正常工作

### 8. 提交

暂存所有已修改/已创建的文件，并以如下消息提交：`Add pre-commit hooks (husky + lint-staged + prettier)`

这会触发新的 pre-commit 钩子——是验证一切正常的良好冒烟测试。

## 注意事项

- Husky v9+ 在钩子文件中无需 shebang
- `prettier --ignore-unknown` 会跳过 Prettier 无法解析的文件（图片等）
- pre-commit 先运行 lint-staged（快速、仅针对暂存文件），再运行完整的类型检查与测试
