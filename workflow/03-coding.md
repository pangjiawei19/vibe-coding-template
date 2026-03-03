# TDD 实战

## 1. 编写测试（RED）

```markdown
请严格遵循TDD流程，**先不要实现功能代码**。

执行任务 **T036 到 T041**：
在 `internal/parser/` 目录下创建 `parser_test.go` 文件。

请根据`plan.md`中的接口定义，为 `Parse` 函数编写一组**表格驱动测试（Table-Driven Tests）**。测试用例必须覆盖以下场景：
1.  合法的 Issue URL。
2.  合法的 Pull Request URL。
3.  合法的 Discussion URL。
4.  无效的 URL（格式错误）。
5.  不支持的 URL 类型（如仓库主页）。

此时 `parser.go` 中可能还没有 `Parse` 函数，或者它是空的。请确保测试代码能够通过编译（你可以先在 `parser.go` 中生成一个空的函数签名），但**运行测试必须失败**。
```

## 2. 实现逻辑（GREEN）

```markdown
测试已失败，正如预期。

现在执行任务 **T043**：在 `internal/parser/parser.go` 中实现 `Parse` 函数的核心逻辑。

**要求：**
1.  逻辑必须能通过刚才编写的所有测试用例。
2.  使用字符串分割（`strings.Split`）来解析URL，**遵循“简单性原则”，尽量避免复杂的正则表达式**。
3.  严格遵循 `spec.md` 中定义的URL模式。
4.  不要过度设计，只写能通过测试的代码。
```

## 3. 在测试的保护下进行重构（按需）

```markdown
代码通过了测试，但我发现 `Parse` 函数有点长，不够整洁。

请进行重构：
1.  将URL路径分割和基础验证逻辑，提取为一个私有的辅助函数 `splitAndValidatePath`。
2.  确保所有错误信息都统一使用 `fmt.Errorf` 包装，保持风格一致。

重构完成后，**必须**再次运行测试，确保没有破坏现有逻辑。
```
