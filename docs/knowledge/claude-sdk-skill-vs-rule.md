# Claude Agent SDK: Skill 与 Rule 概念详解

## 概述

在 Claude Agent SDK 中，**Skill（技能）** 和 **Rule（规则）** 是两个核心概念，它们共同定义了 AI Agent 的能力边界和行为模式。理解这两个概念的区别对于有效使用和配置 Claude Agent 至关重要。

---

## Skill（技能）

### 定义

**Skill** 是 Agent 可执行的专业能力模块，代表 Agent "能做什么"。每个 Skill 封装了特定领域的功能，用户可以通过斜杠命令（slash command）来调用。

### 核心特点

- **可调用性**：通过 `/skill-name` 格式的斜杠命令触发
- **专业化**：每个 Skill 专注于特定任务领域
- **工具集成**：Skill 内部可以调用各种工具（Tools）来完成任务
- **独立性**：不同 Skill 之间相互独立，可按需组合

### 常见 Skill 示例

| Skill 名称 | 斜杠命令 | 功能描述 |
|-----------|---------|---------|
| commit | `/commit` | 生成规范的 Git 提交信息并执行提交 |
| review-pr | `/review-pr` | 审查 Pull Request，提供代码评审意见 |
| pdf | `/pdf` | 处理和分析 PDF 文档 |
| keybindings-help | `/keybindings-help` | 帮助用户自定义键盘快捷键 |

### 使用方式

```
用户: /commit

Agent: [调用 commit skill，分析代码变更，生成提交信息]
```

### Skill 的组成结构

一个典型的 Skill 包含：

1. **名称（name）**：唯一标识符
2. **描述（description）**：说明 Skill 的用途和触发条件
3. **提示词（prompt）**：定义 Skill 执行时的具体行为
4. **工具权限**：Skill 可以使用的工具列表

---

## Rule（规则）

### 定义

**Rule** 是 Agent 的行为约束和指导原则，代表 Agent "该怎么做"。Rule 定义了 Agent 在执行任务时必须遵循的规范和限制。

### 核心特点

- **约束性**：限定 Agent 的行为边界
- **全局性**：Rule 通常在整个会话中持续生效
- **优先级**：Rule 的约束优先于一般行为
- **安全性**：防止 Agent 执行不当操作

### Rule 的类型

#### 1. 系统级 Rule
由 SDK 或平台预设的基础规则，确保安全和合规。

```
示例：
- 不执行可能造成数据丢失的危险操作
- 在执行破坏性命令前必须确认
- 文件系统访问限制在指定目录内
```

#### 2. 项目级 Rule
针对特定项目或代码库的规则，定义在 `.claude/` 配置目录中。

```
示例：
- 遵循项目的代码风格指南
- 使用指定的测试框架
- 提交信息必须符合 Conventional Commits 规范
```

#### 3. 会话级 Rule
在特定会话中临时生效的规则。

```
示例：
- 本次会话只修改前端代码
- 所有响应使用中文
```

### Rule 配置示例

```json
{
  "rules": [
    {
      "name": "code-style",
      "description": "遵循项目代码风格",
      "content": "使用 TypeScript，遵循 ESLint 配置，使用 2 空格缩进"
    },
    {
      "name": "commit-convention",
      "description": "提交信息规范",
      "content": "使用 Conventional Commits 格式：type(scope): description"
    }
  ]
}
```

---

## Skill vs Rule：核心区别

| 维度 | Skill（技能） | Rule（规则） |
|-----|--------------|-------------|
| **本质** | 能力（Capability） | 约束（Constraint） |
| **回答的问题** | Agent 能做什么？ | Agent 该怎么做？ |
| **触发方式** | 用户主动调用（斜杠命令） | 自动生效（持续约束） |
| **作用范围** | 特定任务 | 全局或指定范围 |
| **可见性** | 用户可见并可选择 | 通常在后台运行 |
| **灵活性** | 按需使用 | 强制遵守 |

### 类比理解

可以将 Agent 比作一个专业工程师：

- **Skill** = 工程师掌握的技能（会写代码、会做代码审查、会写文档）
- **Rule** = 工程师必须遵守的规范（代码风格指南、安全规范、工作流程）

工程师可以选择使用哪些技能来完成任务，但在执行过程中必须遵守所有适用的规范。

---

## 协同工作示例

### 场景：执行代码提交

```
用户: /commit

[Skill 激活: commit]
- 分析暂存区的代码变更
- 生成提交信息

[Rule 约束生效]
- Rule: commit-convention → 提交信息必须符合 Conventional Commits 格式
- Rule: code-review → 提交前检查是否有明显问题

[最终输出]
feat(auth): 添加用户登录功能

- 实现登录表单组件
- 添加 JWT token 处理
- 集成后端认证 API
```

在这个例子中：
- **Skill（commit）** 提供了生成提交信息的能力
- **Rule（commit-convention）** 确保提交信息符合规范格式

---

## 最佳实践

### 配置 Skill

1. **明确用途**：每个 Skill 应有清晰的单一职责
2. **提供示例**：在描述中包含使用示例
3. **定义边界**：明确 Skill 能做和不能做的事情

### 配置 Rule

1. **简洁明确**：Rule 描述应清晰无歧义
2. **合理优先级**：重要的安全规则应有更高优先级
3. **避免冲突**：确保不同 Rule 之间不会产生矛盾
4. **定期审查**：随项目演进更新 Rule 配置

---

## 总结

| 概念 | 一句话总结 |
|-----|----------|
| **Skill** | Agent 的专业能力，用户通过斜杠命令按需调用 |
| **Rule** | Agent 的行为规范，在执行过程中自动约束 |

理解 Skill 和 Rule 的区别，有助于：
- 更有效地配置和使用 Claude Agent
- 构建符合项目需求的 Agent 工作流
- 确保 Agent 行为的可预测性和安全性

---

## 参考资料

- [Claude Agent SDK 官方文档](https://docs.anthropic.com)
- [Anthropic Claude 技术博客](https://www.anthropic.com/blog)
