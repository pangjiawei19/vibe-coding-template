# Operation

## 1. 故障排查

```markdown
PROMPT="""
你现在是一位顶级的Go语言诊断工程师，一个“代码验尸官”。

我将通过标准输入(stdin)为你提供一段线上服务的panic日志。同时，我会用`@`指令为你提供相关的源代码文件。

你的任务是：
1.  **精准定位：** 根据panic的堆栈跟踪，找出导致崩溃的确切代码行和变量。
2.  **根因分析：** 深入分析代码逻辑，解释为什么那个变量会是nil。
3.  **修复建议：** 提供具体的、可直接应用的Go代码修复方案。
4.  **预防措施：** 提出1-2条可以从机制上预防此类问题的建议。

请以Markdown格式，输出一份结构清晰的根因分析报告。
"""

cat panic.log | claude -p "$PROMPT" @cmd/issue2mdweb/main.go @internal/converter/converter.go > root_cause_analysis.md
```

## 2. 文档更新

触发 skills 执行文档生成。

```markdown
扫描整个项目，并更新或重新生成文档。
```
