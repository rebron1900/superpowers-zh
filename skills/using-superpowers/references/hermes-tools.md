# Hermes Agent 工具映射

技能使用 Claude Code 的工具名称。当你在技能中遇到这些工具时，使用你平台的等价工具：

| 技能中引用的工具 | Hermes Agent 等价工具 |
|-----------------|----------------------|
| `Read`（读取文件） | `read_file` |
| `Write`（创建文件） | `write_file` |
| `Edit`（编辑文件） | `patch` |
| `Bash`（运行命令） | `terminal` |
| `Grep`（搜索文件内容） | `search_files` |
| `Glob`（按名称搜索文件） | `search_files` |
| `Skill` 工具（调用技能） | `skill_view` |
| `WebFetch` | `web_extract` |
| `WebSearch` | `web_search` |
| `Task` 工具（分派子智能体） | `delegate_task` |
| 多个 `Task` 调用（并行） | 多个 `delegate_task` 调用 |
| `TodoWrite`（任务跟踪） | `todo` |
| `EnterPlanMode` / `ExitPlanMode` | 无等价工具 — 留在主会话中 |

## 技能管理

Hermes Agent 使用三级渐进式技能加载：

| 操作 | 工具 |
|------|------|
| 列出所有可用技能 | `skills_list` |
| 查看技能完整内容 | `skill_view(name)` |
| 查看技能的引用文件 | `skill_view(name, path)` |
| 管理技能（安装/更新） | `skill_manage` |

## CodeGraph 代码图谱

CodeGraph 提供代码库的语义分析能力，基于 tree-sitter 构建知识图谱。

| 功能 | 工具 | 说明 |
|------|------|------|
| 搜索符号 | `mcp_codegraph_codegraph_search` | 按名称搜索函数、类、变量等 |
| 获取上下文 | `mcp_codegraph_codegraph_context` | **主要工具** — 获取入口点、关联符号、关键代码 |
| 探索代码 | `mcp_codegraph_codegraph_explore` | 按文件分组查看多个符号的源码 |
| 符号详情 | `mcp_codegraph_codegraph_node` | 查看单个符号的位置、签名、调用链 |
| 追踪调用链 | `mcp_codegraph_codegraph_trace` | 追踪两个符号之间的调用路径 |
| 查看调用者 | `mcp_codegraph_codegraph_callers` | 列出调用某符号的函数 |
| 查看被调用者 | `mcp_codegraph_codegraph_callees` | 列出某符号调用的函数 |
| 影响分析 | `mcp_codegraph_codegraph_impact` | 修改某符号会影响哪些代码 |
| 文件树 | `mcp_codegraph_codegraph_files` | 查看项目文件结构和语言统计 |
| 状态检查 | `mcp_codegraph_codegraph_status` | 检查索引健康状态 |

**使用场景：**
- 理解代码架构 → `codegraph_context`
- 调试调用链 → `codegraph_trace`
- 重构前评估影响 → `codegraph_impact`
- 快速查找符号 → `codegraph_search`

## Mnemosyne 记忆系统

Mnemosyne 是 Hermes 的持久化记忆系统，支持向量搜索、全文检索、知识图谱。

### 基础记忆操作

| 功能 | 工具 | 说明 |
|------|------|------|
| 存储记忆 | `mnemosyne_remember` | 保存事实、偏好、洞察（支持 importance/veracity 标记） |
| 搜索记忆 | `mnemosyne_recall` | 混合检索：向量相似度 + 全文搜索 + 重要性权重 |
| 存储权威事实 | `mnemosyne_remember_canonical` | 存储单一来源的事实（如身份、固定偏好） |
| 读取权威事实 | `mnemosyne_recall_canonical` | 读取权威事实，支持按 category/name 查询 |
| 更新记忆 | `mnemosyne_update` | 更新已有记忆的内容或重要性 |
| 删除记忆 | `mnemosyne_forget` | 永久删除记忆 |

### 知识图谱（三元组）

| 功能 | 工具 | 说明 |
|------|------|------|
| 添加关系 | `mnemosyne_triple_add` | 添加 (主语, 谓语, 宾语) 关系 |
| 查询关系 | `mnemosyne_triple_query` | 按主语/谓语/宾语查询关系 |
| 结束关系 | `mnemosyne_triple_end` | 标记关系已结束（不删除历史） |

### Persona 层（长期记忆）

| 功能 | 工具 | 说明 |
|------|------|------|
| 提升为长期记忆 | `mnemosyne_persona_promote` | 将工作记忆提升到 persona 层 |
| 列出 persona | `mnemosyne_persona_list` | 查看所有 persona 记忆 |
| 强化记忆 | `mnemosyne_persona_reinforce` | 增加记忆的强化计数 |
| 降级记忆 | `mnemosyne_persona_demote` | 从 persona 层移回工作记忆 |

### 记忆维护

| 功能 | 工具 | 说明 |
|------|------|------|
| 整合记忆 | `mnemosyne_sleep` | 压缩旧工作记忆为摘要 |
| 统计信息 | `mnemosyne_stats` | 查看记忆数量和 BEAM 分层 |
| 诊断 | `mnemosyne_diagnose` | 检查记忆系统健康状态 |
| 导出 | `mnemosyne_export` | 导出所有记忆为 JSON |
| 导入 | `mnemosyne_import` | 从 JSON 或其他提供商导入记忆 |

**使用场景：**
- 保存用户偏好 → `mnemosyne_remember(importance=0.8)`
- 查询历史上下文 → `mnemosyne_recall`
- 建立实体关系 → `mnemosyne_triple_add`
- 重要知识固化 → `mnemosyne_persona_promote`

## Obsidian 笔记库（FNS）

通过 MCP 协议访问 Obsidian 笔记库。

| 功能 | 工具 | 说明 |
|------|------|------|
| 读取笔记 | `mcp_fns_note_get` | 获取笔记完整内容 |
| 创建/更新笔记 | `mcp_fns_note_create_or_update` | 创建或更新笔记 |
| 搜索笔记 | `mcp_fns_note_list` | 按关键词搜索笔记 |
| 添加内容 | `mcp_fns_note_append` / `mcp_fns_note_prepend` | 在笔记首尾添加内容 |
| 替换内容 | `mcp_fns_note_replace` | 查找替换笔记内容 |
| 管理 Vault | `mcp_fns_vault_*` | Vault 的增删改查 |

## 数据库（MSSQL）

| 功能 | 工具 | 说明 |
|------|------|------|
| 执行 SQL | `mcp_mssql_execute_sql` | 执行查询，支持 table/csv/json 格式 |
| 表结构 | `mcp_mssql_describe_table` | 查看列名、类型、可空性 |
| 列出表 | `mcp_mssql_list_tables` | 列出所有表和视图 |

## Excel 操作

| 功能 | 工具 | 说明 |
|------|------|------|
| 读取数据 | `mcp_excel_read_data_from_excel` | 读取单元格数据和验证规则 |
| 写入数据 | `mcp_excel_write_data_to_excel` | 写入数据到工作表 |
| 创建工作簿 | `mcp_excel_create_workbook` | 创建新的 Excel 文件 |
| 格式化 | `mcp_excel_format_range` | 设置单元格格式 |

## 其他 MCP 工具

| 工具 | 用途 |
|------|------|
| `mcp_lego_*` | Lego API 网关管理 |
| `mcp_anx_reader_*` | 静读天下阅读器书架 |
| `mcp_anki_*` | Anki 闪卡管理 |
| `mcp_deepwiki_*` | GitHub 仓库文档查询 |

## 额外的 Hermes Agent 工具

| 工具 | 用途 |
|------|---------|
| `memory` | 持久化知识供未来会话使用（推荐使用 Mnemosyne） |
| `session_search` | 搜索历史会话记录 |
| `execute_code` | 在沙箱中执行代码 |
| `process` | 后台进程管理 |
| `vision_analyze` | 图像分析 |
| `image_generate` | 图像生成 |
| `clarify` | 向用户提出澄清性问题 |
| `browser_*` | 浏览器自动化工具集 |
| `mixture_of_agents` | 多智能体高级推理 |
