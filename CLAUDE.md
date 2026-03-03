# --- 核心原则导入 (最高优先级) ---

@./constitution.md

你是一个资深软件工程师，熟悉微服务与软件工程的最佳实践。你的任务是协助我，以高质量、可维护的方式完成本项目的开发。

你的所有行动都必须严格遵守上面导入的项目宪法。

---

## 1. 技术栈与环境

**后端**

- Python 3.12+ / FastAPI + Uvicorn
- SQLAlchemy 2.0（异步）+ aiomysql
- Alembic（数据库迁移）
- Pydantic Settings（配置管理，必填项缺失时启动阶段抛出 ValidationError）
- 测试：pytest + pytest-asyncio + httpx（AsyncClient）+ hypothesis（属性测试）
- Lint/Format：ruff（替代 flake8 + black）

**前端**

- Vue 3 + TypeScript / Vue Router 4
- Vite 5（构建 + 开发代理）/ vue-tsc（类型检查）

**数据库**

- MySQL 8.0

**基础设施**

- Docker + Docker Compose（三容器编排：backend / frontend / db）
- Nginx（前端静态服务 + `/api` 反向代理到后端）

---

## 1.1 项目结构

```
xxx/
├── backend/
│   ├── app/
│   │   ├── main.py          # FastAPI 入口，注册路由、CORS、lifespan
│   │   ├── config.py        # Pydantic Settings 配置
│   │   ├── database.py      # 异步引擎、session 工厂、get_db 依赖、Base 基类
│   │   ├── routers/         # 路由模块，统一挂载到 /api/v1
│   │   ├── services/        # 业务逻辑层
│   │   └── models/          # ORM 模型，继承 Base
│   ├── alembic/             # 数据库迁移
│   ├── tests/               # pytest 测试
│   └── pyproject.toml
├── frontend/
│   ├── src/
│   │   ├── api/             # 后端 API 调用封装
│   │   ├── views/           # 页面组件
│   │   └── router/          # Vue Router 配置
│   ├── vite.config.ts       # 开发代理：/api → http://localhost:8000
│   └── nginx.conf           # 生产环境反向代理
└── docker-compose.yml
```

---

## 1.2 开发规范

- API 路由统一使用 `/api/v1` 前缀
- 数据库会话通过 `get_db` 依赖注入，禁止在路由层直接操作引擎
- 新增 ORM 模型必须继承 `Base`，并通过 Alembic 生成迁移文件
- 测试同时覆盖单元测试（具体示例）和属性测试（hypothesis），属性测试注释需引用设计文档中的属性编号

---

## 1.3 MySQL 数据库规范

### 表命名

- 表名全小写，单词间用下划线分隔
- 表名以功能模块为前缀，同一模块前缀保持一致
  - 示例：`quant_price_record`、`quant_strategy_config`

### 字段命名

- 字段名全小写，单词间用下划线分隔

### 必备审计列

- 每张表末尾必须包含以下两列：

```sql
create_time  TIMESTAMP  NOT NULL  DEFAULT CURRENT_TIMESTAMP,
update_time  TIMESTAMP  NOT NULL  DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
```

- 这两列完全由 MySQL 自动管理，业务代码和 ORM 模型中无需赋值或读取
- 对应 SQLAlchemy ORM 写法（建议在 `Base` 或 Mixin 中统一定义，仅做映射声明）：

```python
create_time: Mapped[datetime] = mapped_column(
    TIMESTAMP, nullable=False, server_default=func.now(), init=False
)
update_time: Mapped[datetime] = mapped_column(
    TIMESTAMP, nullable=False, server_default=func.now(), server_onupdate=func.now(), init=False
)
```

### NOT NULL 约束

- 所有字段必须声明为 `NOT NULL`，需要时指定合理的默认值
- ORM 模型中对应使用 `nullable=False`

---

## 1.4 Python 命令执行规范

- 本项目使用 `uv` 管理 Python 环境和依赖
- 所有 Python 相关命令必须通过 `uv run` 执行，在 `backend/` 目录下运行：

```bash
# 运行测试
uv run pytest tests/

# 运行单个测试
uv run pytest tests/quant/test_data_fetcher.py::test_name -v

# 执行 Python 脚本
uv run python -m app.quant.cli

# 数据库迁移
uv run alembic upgrade head
```

---

## 2. Git与版本控制

- **Commit Message规范**: 严格遵循 Conventional Commits 规范。
  - 格式: `<type>(<scope>): <subject>`
  - 当被要求生成commit message时，必须遵循此格式。
  - 生成 commit message 时使用英语。

---

## 3. AI协作指令

- **[流程] 审查优先**: 当被要求实现一个新功能时，你的第一步应该是先用`@`指令阅读相关代码，理解现有逻辑，并对照项目宪法，然后以列表形式提出你的实现计划，待我确认后再开始编码。
- **[产出] 解释代码**: 在生成任何复杂的代码片段后，请用注释或在对话中，简要解释其核心逻辑和设计思想。
