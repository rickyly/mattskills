# 重构候选项

TDD 循环之后，寻找：

- **重复（Duplication）** → 提取函数 / 类
- **过长的方法（Long methods）** → 拆成私有辅助方法（测试仍放在公共接口上）
- **浅模块（Shallow modules）** → 合并或深化
- **特性依恋（Feature envy）** → 把逻辑移到数据所在之处
- **基本类型偏执（Primitive obsession）** → 引入值对象
- **现有代码** 被新代码揭示出存在问题
