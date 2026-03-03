# Plan & Task

## 1. 生成技术方案

```markdown
`@./specs/001-core-functionality/spec.md`

你现在是`xxx`项目的首席架构师。你的任务是基于我提供的`spec.md`以及我们已有的`constitution.md`，为项目生成一份详细的技术实现方案（`plan.md`）。

**技术栈约束 (必须遵循):**

- **语言**: Go (>=1.21.0)
- **Web框架**: 仅使用标准库 `net/http`，不引入Gin或Echo等外部框架（遵循“简单性原则”）。
- **GitHub API客户端**: 使用 `google/go-github` 库，结合GraphQL API v4。
- **Markdown输出处理**: 尽量不使用任何第三方库。
- **数据存储**: 本项目初期**不需要数据库**，所有数据通过API实时获取。

**方案内容要求 (必须包含):**

1.  **技术上下文总结:** 明确上述技术选型。
2.  **“合宪性”审查:** 逐条对照`constitution.md`的原则，检查并确认本技术方案符合所有条款（特别是包内聚、错误处理、TDD）。
3.  **项目结构细化:** 明确各包的具体职责和依赖关系。
4.  **核心数据结构:** 定义在模块间流转的核心数据结构，必须包含Spec中要求的所有字段。
5.  其他

请严格按照`@./.claude/templates/plan-template.md`的模板格式来组织你的输出（如果模板不存在，请自行设计一个结构清晰的Markdown格式）。

完成后，将生成的`plan.md`内容写入到`./specs/001-core-functionality/plan.md`文件中。
```

重要：**审查 spec.md**

## 2. 任务拆解

```markdown
方案非常完美。

现在，请扮演技术组长。请仔细阅读 `@./specs/001-core-functionality/spec.md` 和 `@./specs/001-core-functionality/plan.md`。

你的目标是将 `plan.md`中描述的技术方案，分解成一个**详尽的、原子化的、有依赖关系的、可被AI直接执行的任务列表**。

**关键要求：**
1.  **任务粒度：** 每个任务应该只涉及一个主要文件的修改或创建一个新文件。不要出现“实现所有功能”这种大任务。
2.  **TDD强制：** 根据`constitution.md`的“测试先行铁律”，**必须**先生成测试任务，后生成实现任务。
3.  **并行标记：** 对于没有依赖关系的任务，请标记 `[P]`。
4.  **阶段划分：** 即便`plan.md`中包含了粗略的阶段划分，也要以下面的为准。
    *   **Phase 1: Foundation** (数据结构定义)
    *   **Phase 2: Module1** (核心模块1，TDD)
    *   **Phase 3: Module2** (核心模块2，TDD)

完成后，将生成的任务列表写入到`./specs/001-core-functionality/tasks.md`文件中。
```

重要：**审查 task.md**
