# Review

## 1. code review

触发 skills 执行 code review

## 2. commit

```markdown
1. 执行 `git diff --staged` 获取暂存区的变更。
2. 根据变更内容，生成一条严格遵循 **Conventional Commits** 规范的 Commit Message。
3. 向用户展示生成的 Message，并询问是否确认提交。
4. 如果确认，执行 `git commit -m "..."`。
```
