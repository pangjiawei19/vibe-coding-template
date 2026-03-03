# Build

## 1. Dockerfile

```markdown
你现在是一位资深的DevOps工程师，专精于Go应用的容器化。

请为我们的项目，编写一个`Dockerfile`。如果`Dockerfile`已存在，请审查并重写我们项目现有的`Dockerfile`。

**必须遵循以下生产级最佳实践：**

1.  **多阶段构建 (Multi-stage Build):**
    *   使用一个包含完整Go工具链的`builder`阶段来编译应用。
    *   使用一个极度精简的`alpine`或`distroless`镜像作为最终的`final`阶段，只拷贝编译好的二进制文件，以实现最小化的镜像体积。
2.  **依赖缓存:** 优化`go mod`相关指令的顺序，确保依赖层能够被Docker有效缓存，加速后续构建。
3.  **安全性:**
    *   在最终阶段，使用一个非root用户来运行应用。
    *   确保最终镜像不包含任何源代码或构建工具。

请分析我们项目的结构（`@.`），特别是`cmd/issue2md/main.go`和`cmd/issue2mdweb/main.go`这两个入口，然后生成这份优化后的`Dockerfile`，并覆盖写入。
```

## 2. 封装 slash command

像之前的 commit、docker build 等命令，让 AI 在执行时都要用类似的 prompt，可以把这些都封装成 slash command，参考：

```markdown
请帮我在创建一个自定义的项目级的slash command文件 `.claude/commands/build.md`。

这个指令的作用是：调用`make build`来构建项目。如果构建失败，它应该自动分析错误原因。

`allowed-tools` 应该包含 `Bash(make:build)`。
```

## 3. 集成 CI

例如生成 Github Actions：

```markdown
非常棒！最后一步，我们需要为这个项目设置一套CI/CD流水线。

请为我们创建一个GitHub Actions工作流配置文件，路径为`.github/workflows/ci.yml`。

**这个流水线需要实现以下功能：**
1.  **触发条件：** 当有代码被推送到`main`分支，或者有新的Pull Request被创建时触发。
2.  **核心任务：** 在一个Ubuntu环境中，它需要依次执行以下步骤：
    *   Checkout代码。
    *   设置Go环境。
    *   运行`make test`。
    *   运行`make lint`。
    *   运行`make build`。
3.  **健壮性：** 确保任何一步失败，整个流水线都会失败。
```
