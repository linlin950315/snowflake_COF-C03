# COF-C03 (SnowPro Core) 官方文档学习清单

## 各单元题目占比（题库共 116 题）

统计口径：主要标签和次要标签都算，同一题里同一单元只算一次。19 题涉及多个单元，所以单元出现次数合计 135。

| 单元 | 涉及题数 | 占 116 题比例 | 占 135 次总数比例 |
|---|---|---|---|
| S2 | 34 | 29.3% | 25.2% |
| S6 | 21 | 18.1% | 15.6% |
| S9 | 20 | 17.2% | 14.8% |
| S4 | 19 | 16.4% | 14.1% |
| S7 | 16 | 13.8% | 11.9% |
| S5 | 14 | 12.1% | 10.4% |
| S3 | 6 | 5.2% | 4.4% |
| S8 | 5 | 4.3% | 3.7% |

- 第三列因为有跨单元的题，加起来超过 100%，看"这个单元的知识点会涉及多少题"。
- 第四列加起来是 100%，看"各单元的相对权重"。

---

按 Udemy 课程章节划分。⭐⭐⭐ 必读 / ⭐⭐ 重点 / ⭐ 补充

> 各章末尾的「本章题目」表来自题目索引（还需 = 做题前要先学的其他章节）。

> 注：部分链接（Row Access、Query Profile、Semi-structured、UDF 等）非 Guides 首页直链，如打不开请在 docs.snowflake.com 搜索标题。

---

## セクション2: Snowflake AI Data Cloud Features and Architecture

按课程每一讲对应官方文档。

### 架构（6～10 讲）

**架构笔记（三层结构）**

Snowflake 从高到低分三层：

- **Cloud Services Layer（服务层）**：最上层，管理云服务、安全认证、元数据
- **Query Processing Layer（查询处理层 / 虚拟仓库层）**：中间层，负责计算与查询执行。特点：可动态（dynamically）resize；各虚拟仓库相互隔离，互不影响；每个仓库可单独配置大小；支持自动挂起（auto-suspend）与自动恢复（auto-resume）
- **Storage Layer（存储层）**：最下层，可存储多种数据类型（结构化、半结构化、非结构化）

- [x] **6. What is Snowflake? ／ 7. Multi-cluster Shared Data Architecture ／ 10. Services Layer Overview**
- ⭐⭐⭐ [Key Concepts & Architecture](https://docs.snowflake.com/en/user-guide/intro-key-concepts) — 三层架构、存算分离、shared-disk + shared-nothing
- ⭐⭐ [Supported Cloud Platforms](https://docs.snowflake.com/en/user-guide/intro-cloud-platforms) — AWS / Azure / GCP
- ⭐ [Supported Regions](https://docs.snowflake.com/en/user-guide/intro-regions)
- 📝 [Q2](题库.md#q2) Cloud Services 层职责

- [x] **8. Storage Layer Overview**
- ⭐⭐⭐ [Micro-partitions & Data Clustering](https://docs.snowflake.com/en/user-guide/tables-clustering-micropartitions) — 列式存储、微分区 50～500MB（未压缩）
- 📝 [Q91](题库.md#q91) 微分区元数据（还需 S6）

- [x] **9. Query Processing Layer Overview**
- ⭐⭐⭐ [Overview of Warehouses](https://docs.snowflake.com/en/user-guide/warehouses-overview) — 虚拟仓库就是计算层（セクション5 会细讲）
- 📝 [Q13](题库.md#q13) Query Processing 层特征

### 版本与对象模型（11～17 讲）
- [x] **11. Snowflake Editions**

**版本笔记**

- **Standard**：1 天 Time Travel
- **Enterprise**：90 天 Time Travel；多集群仓库
- **Business Critical**：适用银行、医疗等行业（HIPAA / PCI DSS 合规）；更强的加密与敏感数据保护
- **VPS（Virtual Private Snowflake）**：最高级版，适用政府和金融机构；完全独立环境，元数据不在 Cloud Services 层与其他客户共享

- ⭐⭐⭐ [Snowflake Editions](https://docs.snowflake.com/en/user-guide/intro-editions) — 哪个功能从哪个版本开始有（如多集群仓库、90 天 Time Travel 需 Enterprise）
- 📝 [Q4](题库.md#q4) 90天 Time Travel 所需版本（还需 S9）

- [x] **12. Snowflake Object Model**
- ⭐⭐ [Databases, Tables & Views](https://docs.snowflake.com/en/guides-overview-db) — Organization → Account → Database → Schema → 对象
- ⭐ [Securable Objects](https://docs.snowflake.com/en/user-guide/security-access-control-overview#securable-objects) — 对象层级图

- [x] **13. Organizations, Accounts, Databases & Schemas**
- ⭐⭐ [Organizations](https://docs.snowflake.com/en/user-guide/organizations) — ORGADMIN
- ⭐⭐ [Account identifiers](https://docs.snowflake.com/en/user-guide/admin-account-identifier) — orgname-accountname / account locator
- 📝 [Q103](题库.md#q103) 业务隔离：多账号 ⚠️

- [x] **14. Table Types**

**表类型笔记**

`SHOW TABLES` 结果中有 `kind` 列，显示表的类型：

| kind 值 | 类型 | Time Travel | Fail-safe |
|---|---|---|---|
| `TABLE` | Permanent（永久表） | Standard 0～1 天；Enterprise 起 0～90 天 | 7 天 |
| `TRANSIENT` | Transient（跨会话临时表） | 0～1 天（默认 1） | 无 |
| `TEMPORARY` | Temporary（仅当前会话） | 0～1 天（默认 1） | 无 |

注意：Permanent 的 kind 值是 `TABLE` 而不是 `PERMANENT`。原因是 Permanent 是 Snowflake 最初唯一的表类型，就叫"TABLE"；Transient / Temporary 是后来加的特殊变体，才有了自己的名字。`CREATE TABLE` 不写修饰词默认就是 Permanent，`kind` 列也同理显示 `TABLE`。

- ⭐⭐⭐ [Temporary and Transient Tables](https://docs.snowflake.com/en/user-guide/tables-temp-transient) — Permanent / Transient / Temporary 对比
- ⭐⭐ [External Tables](https://docs.snowflake.com/en/user-guide/tables-external-intro)
- ⭐ [Hybrid Tables](https://docs.snowflake.com/en/user-guide/tables-hybrid)
- 📝 [Q6](题库.md#q6) 外部表的存储方式
- 📝 [Q19](题库.md#q19) Transient 表降低 CDP 存储成本（还需 S9）

- [x] **15. View Types**
- ⭐⭐⭐ [Overview of Views](https://docs.snowflake.com/en/user-guide/views-introduction) — 普通 / Secure / Materialized
- ⭐⭐ [Secure Views](https://docs.snowflake.com/en/user-guide/views-secure)
- ⭐⭐ [Materialized Views](https://docs.snowflake.com/en/user-guide/views-materialized) — 需 Enterprise、单表、后台维护有费用

- [x] **16. Update: Worksheets vs. Workspaces**
- ⭐⭐ [Snowsight](https://docs.snowflake.com/en/user-guide/ui-snowsight)
- ⭐ [Worksheets](https://docs.snowflake.com/en/user-guide/ui-snowsight-worksheets) — 官网：Legacy Worksheets 计划于 2026-06-22 从 Snowsight 移除，由 Workspaces 取代
- ⭐ [Workspaces](https://docs.snowflake.com/en/user-guide/ui-snowsight/workspaces)
- 📝 [Q7](题库.md#q7) 可写入 Git 仓库的功能（还需 S2-29）

- [x] **17. Databases, Schemas, Tables & Views - Hands-on**
- ⭐⭐ [String & binary data types](https://docs.snowflake.com/en/sql-reference/data-types-text) — CHAR / CHARACTER / STRING / TEXT 都只是 VARCHAR 的同义词，显示时统一为 VARCHAR（Q18 考点）
- ⭐ [CREATE TABLE](https://docs.snowflake.com/en/sql-reference/sql/create-table) / [CREATE VIEW](https://docs.snowflake.com/en/sql-reference/sql/create-view) — 动手时查语法用
- 📝 [Q18](题库.md#q18) CHARACTER 显示为 VARCHAR

### C03 新增（18～20 讲）
- [x] **18. Open Table Formats & Apache Iceberg Table Type**

**Iceberg 笔记**

- Snowflake Open Catalog 是 Apache Iceberg™ 表的目录（Catalog）实现，基于 Iceberg REST Catalog 规范，使 Spark / Trino 等其他引擎也能通过统一接口访问 Iceberg 表
- Iceberg 支持的功能：
  - **ACID transactions**（原子性、一致性、隔离性、持久性）事务
  - **Schema evolution**（模式演化）
  - **Hidden partitioning**（隐藏分区）
  - **Table snapshots**（表格快照）

- ⭐⭐⭐ [Apache Iceberg Tables](https://docs.snowflake.com/en/user-guide/tables-iceberg) — External Volume、Snowflake 管理 vs 外部 Catalog
- ⭐ [Snowflake Open Catalog](https://docs.snowflake.com/en/user-guide/opencatalog/overview)
- 📝 [Q8](题库.md#q8) Iceberg 表（湖仓/ACID/Schema 演进）
- 📝 [Q12](题库.md#q12) Open Catalog 与 Iceberg

- [x] **19. Parameters ／ 20. Parameters Hands-on**
- ⭐⭐⭐ [Parameters](https://docs.snowflake.com/en/sql-reference/parameters) — Account / Session / Object 三类，可在不同层级设置并逐级覆盖：Session 参数 Account → User → Session（后者覆盖前者）；Object 参数 Account → 对象（database / schema / table 等，下层对象覆盖上层）。Object 并不排在 Session 之后
- ⭐ [SHOW PARAMETERS](https://docs.snowflake.com/en/sql-reference/sql/show-parameters)
- 📝 [Q16](题库.md#q16) DATA_RETENTION 参数层级继承（还需 S9）

### 函数与过程（21～24 讲）
- [x] **21. User Defined Functions (UDFs)**
- ⭐⭐⭐ [UDF Overview](https://docs.snowflake.com/en/developer-guide/udf/udf-overview) — SQL / JavaScript / Python / Java / Scala，标量 vs 表函数（UDTF）
- ⭐⭐⭐ [SQL UDF 介绍](https://docs.snowflake.com/en/developer-guide/udf/sql/udf-sql-introduction) — SQL UDF 引用（reference）的 schema 对象：表（table）、视图（view）、其他函数（other function）
- 📝 [Q67](题库.md#q67) UDF 可引用的对象（还需 S2-15）

- [ ] **22. Stored Procedures**
- ⭐⭐⭐ [Stored Procedures Overview](https://docs.snowflake.com/en/developer-guide/stored-procedure/stored-procedures-overview) — 与 UDF 的区别、owner's rights / caller's rights

- [ ] **23. UDFs, External Functions & Stored Procedures - Hands-on**
- ⭐⭐ [External Functions](https://docs.snowflake.com/en/sql-reference/external-functions-introduction) — API Integration、调用外部 API

- [ ] **24. Sequences**
- ⭐⭐ [Using Sequences](https://docs.snowflake.com/en/user-guide/querying-sequences) — 不保证无间隙

### 管道（25～26 讲）
- [x] **25. Tasks & Streams**
- ⭐⭐⭐ [Streams](https://docs.snowflake.com/en/user-guide/streams-intro) — Standard / Append-only / Insert-only
  - 常见搭配：Stream 加 Task，做定时增量处理
  - Standard Stream 用来记录对表的增删改
  - Append-only：可建在标准表、(dynamic table、Snowflake 管理的 Iceberg 表、视图上)只记录新增行
  - 地理空间数据（geospatial）：Standard stream 取不到变更数据，建议用 append-only stream
  - Insert-only：，只记录新增。建在 external tables、externally managed Iceberg tables（及无分区列的 Delta Direct 表）上
- ⭐⭐⭐ [Tasks](https://docs.snowflake.com/en/user-guide/tasks-intro) — 定时、DAG、serverless 任务
- 📝 [Q55](题库.md#q55) Stream 特性/失效
- 📝 [Q56](题库.md#q56) Insert-only Stream（还需 S2-14）
- 📝 [Q60](题库.md#q60) Stream + Task 定时处理变更
  - 笔记：Task 通常再加 `WHEN SYSTEM$STREAM_HAS_DATA('stream_name')`，没有新变更时就跳过执行，省 credits。
- 📝 [Q85](题库.md#q85) SCD Type 2：Stream + Task + MERGE（还需 S8）

- [x] **26. Dynamic Tables - Hands-on**
- ⭐⭐⭐ [Dynamic Tables](https://docs.snowflake.com/en/user-guide/dynamic-tables/overview) — target lag、与 Streams+Tasks 对比
- ⭐⭐⭐ [Refresh modes](https://docs.snowflake.com/en/user-guide/dynamic-tables/refresh-modes) — 全量 vs 增量刷新
  - 笔记：创建时的几个关键参数
    - `TARGET_LAG`：数据允许落后源表的最大时长（如 `'1 minute'`、`'1 hour'`），或 `DOWNSTREAM`（由下游依赖它的 DT 的 lag 决定刷新节奏）。
    - `REFRESH_MODE`（官网现有 AUTO / FULL / INCREMENTAL / ADAPTIVE / CUSTOM_INCREMENTAL；ADAPTIVE 默认增量，成本过高时自动重建）：
      - `AUTO`（默认）：创建时 Snowflake 评估一次，选定 INCREMENTAL 或 FULL，之后锁定不再变化；不支持增量的查询会选 FULL。
      - `INCREMENTAL`：只处理源表变更的部分，**更高效（more efficient）**；查询不支持增量时创建报错。
      - `FULL`：每次刷新都重新计算整个查询结果。
    - `INITIALIZE`：首次刷新何时发生。
      - `ON_CREATE`（默认）：CREATE 语句会**阻塞**，直到初次刷新完成，建好即有数据。
      - `ON_SCHEDULE`：CREATE 立即返回，表**保持为空**，直到按调度完成第一次刷新。
- 📝 [Q62](题库.md#q62) Dynamic Table 全量刷新原因
- 📝 [Q112](题库.md#q112) Dynamic Table

### 工具与连接（27～30 讲）
- [x] **27. SnowSQL - Hands-on**
- ⭐⭐ [SnowSQL](https://docs.snowflake.com/en/user-guide/snowsql)
- ⭐ [Snowflake CLI](https://docs.snowflake.com/en/developer-guide/snowflake-cli/index) — 新一代命令行工具
- 📝 [Q1](题库.md#q1) Snowflake CLI vs SnowSQL
  - **SnowSQL**：传统 SQL 客户端，侧重**执行操作**；Q9 用途是**加载和卸载数据**（B）；典型用途：跑 SQL、批量脚本、PUT/GET、COPY 数据加载和卸载；状态：逐步被取代（补充）
  - **Snowflake CLI（`snow`）**：新一代开发者工具，侧重**管理和部署应用**；Q1 主要用于**管理工作负载和应用**（B）；典型用途：开发和部署 Native Apps、Streamlit、Snowpark、Notebook 等，也可以跑 SQL；状态：官方主推的新工具
- 📝 [Q3](题库.md#q3) Snowflake CLI 变量模板语法 ⚠️
- 📝 [Q9](题库.md#q9) SnowSQL 用途
- 📝 [Q17](题库.md#q17) CLI 连接配置优先级（环境变量 > config.toml）
  - 优先级从高到低：**命令行参数 command-line arguments（如 `--role`）> 环境变量 environment variables（如 `SNOWFLAKE_ROLE`）> config.toml**
  - 官网补充（Snowflake CLI 的 Managing connections 页）：① 命令行参数 > ② 覆盖特定 config.toml 参数的环境变量（形如 `SNOWFLAKE_CONNECTIONS_<NAME>_PASSWORD`）> ③ config.toml / connections.toml 中的连接 > ④ 通用环境变量（如 `SNOWFLAKE_USER`）。即只有 ② 高于 config.toml，通用环境变量反而低于它；做题以上面的「环境变量 > config.toml」为准

- [x] **28. IDE Integrations - Visual Studio Code**
- ⭐ [Snowflake Extension for VS Code](https://docs.snowflake.com/en/user-guide/vscode-ext)
  - `connections.toml` 文件：add and modify connection definitions（添加和修改连接定义）
  - 别和 Snowflake CLI 的 `config.toml` 混
- 📝 [Q107](题库.md#q107) VS Code 扩展配置文件

- [ ] **29. Connectors, Drivers & Partnered Tools**
- ⭐⭐⭐ [Applications & tools for connecting](https://docs.snowflake.com/en/guides-overview-connecting)
- ⭐⭐ [Drivers](https://docs.snowflake.com/en/developer-guide/drivers) — JDBC / ODBC / Python / Node.js / Go / .NET
- ⭐ [Snowflake Ecosystem（合作伙伴工具）](https://docs.snowflake.com/en/user-guide/ecosystem)
- ⭐ [Developer](https://docs.snowflake.com/en/developer) — Snowpark、Streamlit、Native Apps
- 📝 [Q44](题库.md#q44) Git 集成需要的对象 ⚠️（还需 S4）

- [ ] **30. Snowflake Scripting**
- ⭐⭐ [Snowflake Scripting](https://docs.snowflake.com/en/developer-guide/snowflake-scripting/index) — BEGIN…END、变量、游标、异常处理

### 计费（31 讲）
- [x] **31. Billing Overview**
- ⭐⭐⭐ [Understanding Overall Cost](https://docs.snowflake.com/en/user-guide/cost-understanding-overall) — 计算 / 存储 / 数据传输；Cloud Services 超过当日计算 10% 才收费
- ⭐⭐ [Cost & Billing](https://docs.snowflake.com/en/guides-overview-cost)
- 📝 [Q37](题库.md#q37) Budgets（还需 S5）
- 📝 [Q38](题库.md#q38) Budget vs Resource Monitor（还需 S5）

## セクション3: Snowflake AI Data Cloud Features and Architecture (AI Features)

按课程每一讲对应官方文档（32～37 讲）。

### 📌 一句话定义速记（依据官方文档，2026-11 核对）

| 功能 | 一句话定义 | 文档里的关键事实 / 注意 |
|---|---|---|
| **Snowpark** | 在 Snowflake 内大规模查询和处理数据的库（DataFrame API） | 支持 **Java、Python、Scala** 三种语言；计算全在 Snowflake 内完成，无需另建集群；惰性执行，调用 `collect()` 等 action 时才发送 SQL |
| **Snowflake Notebooks** | Snowsight 里统一的开发界面，单元格式环境，可写 Python、SQL、Markdown | 单元格类型：SQL / Python / Markdown；SQL 和 Snowpark 查询始终在 warehouse 上运行；⚠️ 文档已把原版改名 **Legacy Notebooks**，新一代是 Notebooks in Workspaces |
| **Streamlit in Snowflake** | 帮开发者在 Snowflake 上安全地构建、部署、分享 Streamlit 应用 | Streamlit 本身是开源 Python 库；底层计算和存储由 Snowflake 管理；代码存在 Snowflake 对象里，用 RBAC 控制访问 |
| **Cortex AI Functions (AISQL)** | 用 SQL（也可用 Python）直接调用 LLM，对文本和图片做非结构化分析 | 需要 `USE AI FUNCTIONS` 权限 + `CORTEX_USER` 或 `AI_FUNCTIONS_USER` 数据库角色；为吞吐量优化，适合批处理；旧的 `SNOWFLAKE.CORTEX.*` 函数是旧名 |
| **Snowflake Copilot** | 由 LLM 驱动的助手，简化数据分析并保持数据治理 | 看不到表里的数据，只看元数据；不支持跨数据库或跨 schema 查询；权限靠 `COPILOT_USER` 角色（默认授予 PUBLIC）；⚠️ 文档顶部提示它**正被 Cortex Code 取代** |
| **Document AI / Cortex AI Functions: Documents** | 以 Cortex AI 函数形式提供的文档智能：解析文档、提取结构化信息、分类文档 | 核心函数：`AI_PARSE_DOCUMENT`（解析，保留版面）、`AI_EXTRACT`（按 schema 提取）、`AI_CLASSIFY`（分类）；原 Document AI 链接现在指向这一页 |
| **ML Functions** | 用机器学习给数据提供自动预测和洞察，无需 ML 专业知识 | 文档列出四个：**Forecasting**（预测时间序列未来值）、**Anomaly Detection**（标出异常值）、**Classification**（把行分到两个或多个类别）、**Top Insights**（找出对指标有意外影响的维度和值）；训练出的模型实例会产生存储费用 |
| **Cortex Fine-tuning** | 用自有数据定制大语言模型，针对特定任务 | 全托管，用 PEFT（参数高效微调）；训练数据需有 `prompt` 和 `completion` 两列；⚠️ 文档写明**计划 2027 年 Q1 弃用**，改用 Cortex Training |
| **Cortex Search** | 对 Snowflake 数据做低延迟、高质量的"模糊"搜索 | 混合搜索：向量搜索 + 关键词搜索 + 语义重排；用 `CREATE CORTEX SEARCH SERVICE` 创建；常用于 RAG |
| **Cortex Analyst** | 全托管的 LLM 功能，让业务人员用自然语言问结构化数据 | 把自然语言问题转成 SQL，在你自己的 warehouse 上执行；以 REST API 提供；依赖 Semantic View（旧的 YAML 语义模型仍兼容）；⚠️ 文档顶部建议迁移到 Cortex Agents |

**一句话区分（对应 Q14 / Q101 / Q102）：**
- 想**写代码**处理数据 → Snowpark
- 想**做界面** → Streamlit
- 想用现成的 **SQL 函数做预测/异常检测** → ML Functions
- 想**调大模型处理文本/图片** → Cortex AI Functions
- 想**搜非结构化文本** → Cortex Search
- 想用**自然语言查结构化表** → Cortex Analyst

> 说明：Copilot、Fine-tuning、Analyst、Notebooks 的文档都带有迁移或弃用提示，但考试题库可能仍按旧名称出题，答题时按题目给出的名称作答。

### 总览
- ⭐⭐⭐ [Snowflake AI & ML](https://docs.snowflake.com/en/guides-overview-ai-features) — Cortex AI Functions、Cortex Search、Cortex Analyst、Document AI、ML Functions
- ⭐ [Snowflake Horizon Catalog](https://docs.snowflake.com/en/user-guide/snowflake-horizon)

### Snowpark 与 Notebooks（32～34 讲）
- [x] **32. Introduction to Snowpark**
- ⭐⭐⭐ [Snowpark API](https://docs.snowflake.com/en/developer-guide/snowpark/index) — 支持 Python / Java / Scala；DataFrame 惰性执行，在 Snowflake 内下推计算
- 📝 [Q15](题库.md#q15) Snowpark 支持语言（S2-29，学完本讲更稳）
- 📝 [Q21](题库.md#q21) 用 Python 连接 Snowflake（S2-29，学完本讲更稳）

- [ ] **33. Introduction to Snowflake Notebooks [COF-C03]**
- ⭐⭐⭐ [Snowflake Notebooks](https://docs.snowflake.com/en/user-guide/ui-snowsight/notebooks) — Snowsight 内的 SQL / Python 交互式开发环境

- [ ] **34. Executing SQL and Python Snowpark in Notebooks [COF-C03]**
- ⭐⭐ [Develop and run code in Snowflake Notebooks](https://docs.snowflake.com/en/user-guide/ui-snowsight/notebooks-develop-run) — SQL 单元格、Python 单元格、Snowpark Session

### Cortex AI（35～36 讲）
- [ ] **35. Snowflake Cortex: AISQL, Copilot & Document AI [COF-C03]**
- ⭐⭐⭐ [Cortex AI Functions (AISQL)](https://docs.snowflake.com/en/user-guide/snowflake-cortex/aisql) — 官网现用 AI_ 前缀：AI_COMPLETE / AI_SUMMARIZE / AI_SENTIMENT / AI_TRANSLATE / AI_EXTRACT / AI_CLASSIFY / AI_FILTER 等（旧名 COMPLETE、SUMMARIZE 等是 SNOWFLAKE.CORTEX.* 旧函数，考试可能用旧名），直接在 SQL 里调 LLM，无需自建模型
- ⭐⭐ [Snowflake Copilot](https://docs.snowflake.com/en/user-guide/snowflake-copilot) — 自然语言辅助写 SQL
- ⭐⭐ [Document AI](https://docs.snowflake.com/en/user-guide/snowflake-cortex/document-ai/overview) — 从文档提取结构化信息（此链接现重定向到 Cortex AI Documents 页，官网主推 AI_EXTRACT 提取文档信息）
- ⭐⭐ [ML Functions](https://docs.snowflake.com/en/guides-overview-ml-functions) — Forecasting、Anomaly Detection、Top Insights（归到本讲为推测）
- 📝 [Q14](题库.md#q14) ML Functions（Top Insights）
- 📝 [Q102](题库.md#q102) Snowflake Cortex

- [ ] **36. Snowflake Cortex: Fine-tuning, Search & Analyst [COF-C03]**
- ⭐⭐ [Cortex Fine-tuning](https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-finetuning) — 用自有数据微调 LLM
- ⭐⭐⭐ [Cortex Search](https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-search/cortex-search-overview) — 非结构化文本的混合（向量 + 关键词）RAG搜索
- ⭐⭐⭐ [Cortex Analyst](https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-analyst) — 业务人员用自然语言问结构化数据，自动生成 SQL（依赖语义模型/semantic view；官网页首提示建议迁移到 Cortex Agents）
- 📝 [Q101](题库.md#q101) Cortex Analyst

### Streamlit（37 讲）
- [ ] **37. Streamlit**
- ⭐⭐⭐ [Streamlit in Snowflake](https://docs.snowflake.com/en/developer-guide/streamlit/about-streamlit) — 在 Snowflake 内创建和分享数据 / ML 的 Web 应用
- 📝 [Q11](题库.md#q11) Streamlit in Snowflake（S2-29，学完本讲更稳）

## セクション4: Account Access & Security

按课程每一讲对应官方文档（截图显示本章 22 讲约 1 小时 55 分，我只看到 38～58 共 21 讲，第 59 讲未见，需补）。题目按讲次归属为推测，以题库内容为准。

### 📌 知识点速记（按官方文档整理，2026-11 核对）

> 按知识本身分 5 个模块：访问控制 → 认证 → 网络与加密 → 数据保护 → 监控与脉络。每个模块先看"概念定义"，再看"易混对比"。末尾只标出对应的题号，方便你学完回头做题。
> 标 ⚠️ 的是文档页面里没写、靠课程或经验补充的内容，建议复核。

#### 模块 1：访问控制（对应 39～43 讲）

**核心模型：** Snowflake 以 RBAC 为主（权限给角色，角色给用户），同时有 DAC（每个对象有 owner，owner 可以授权）。没有"超级用户"，任何访问都需要相应权限，默认拒绝。

| 概念 | 一句话定义 | 要点 |
|---|---|---|
| **Securable object** | 可以被授予访问权限的对象 | 层级：账号 → 数据库 → schema → 表、视图、函数、stage 等 |
| **OWNERSHIP** | 对象的拥有权，可删除、修改、授予或撤销访问 | 一个对象只有一个 owner 角色，默认是创建它的角色；owner 角色**不会**继承它所拥有的角色的权限 |
| **MANAGE GRANTS** | 像 owner 一样对任何对象授予或撤销权限 | 默认只有 SECURITYADMIN 有；有它**不等于能创建对象** |
| **角色层级** | 权限沿层级向上继承（**权限从下往上走**） | Role 3 授给 Role 2，Role 2 授给 Role 1，则 Role 1 拥有三者的权限；`GRANT ROLE X TO ROLE Y`：`TO` 后面的 Y 是上层，拿到 X 的权限，X 拿不到 Y 的 |
| **Primary / Secondary Role** | 一次会话只有 1 个主角色，可同时有多个副角色 | **CREATE 对象只由主角色授权**，新对象的 owner 是主角色；其他操作任一活动角色有权限即可 |
| **USE SECONDARY ROLES** | 设置当前会话的副角色 | `ALL`：每条语句都重新计算，新授予的角色自动生效；`NONE`：只用主角色；指定列表：立即校验，没授予会报错 |
| **Managed Access Schema** | 把授权权限收归 schema owner | 对象 owner **失去**授权能力；只有 schema owner 或有 MANAGE GRANTS 的角色能授权（含 future grants） |
| **Future Grants** | 对将来创建的某类对象预先授权 | 授给**需要访问的角色**；schema 级和 database 级同时存在时，**schema 级优先**，database 级被忽略；`GRANT ... ON ALL TABLES` 只覆盖已有表 |
| **IS_ROLE_IN_SESSION** | 判断某账号角色是否在当前活动的主/副角色层级里 | 会考虑继承；在共享对象里返回 NULL；用于 masking / row access policy 时要求 Enterprise 及以上 |
| **Database Role** | 限定在某个数据库内的角色 | **不能直接在会话中激活**，要授给账号角色；账号角色不能授给 database role；授给 share 后，不能再往它里面授其他 database role |

**系统角色分工：**

| 角色 | 负责什么 |
|---|---|
| *ORGADMIN*（第一层）| 组织层面的操作；文档说将逐步淘汰，推荐 GLOBALORGADMIN |
| **ACCOUNTADMIN（第二层）** | 账号最高角色，包含 SYSADMIN 和 SECURITYADMIN；SNOWFLAKE 库对所有用户可见，但 ACCOUNT_USAGE 等 schema 默认靠 ACCOUNTADMIN 查看，可用 `IMPORTED PRIVILEGES` 或 SNOWFLAKE 数据库角色（如 `USAGE_VIEWER`）下放；只应授给少数人（官方概览页原文）；⚠️ 不要设为默认角色、避免用它创建对象、避免用在自动化脚本中，是通用最佳实践，概览页没有原文 |
| **SYSADMIN（并列第三层）** | 创建仓库、数据库等对象；自定义角色放在它下面时，它才能管理这些角色拥有的对象 |
| **SECURITYADMIN（并列第三层）** | 全局管理授权（有 MANAGE GRANTS privilege） |
| **USERADMIN（SECURITYADMIN 下面）** | 被 SECURITYADMIN 继承（SECURITYADMIN 拥有 USERADMIN 的权限，不是反过来）；创建和管理用户与角色（有 CREATE USER、CREATE ROLE） |
| **PUBLIC** | 自动授予每个用户和每个角色的伪角色，它拥有的对象所有人都能用 |

**谁能管对象的权限（常考表）：**

| 角色 | 普通 schema | Managed access schema |
|---|---|---|
| SYSADMIN | 否 | 否 |
| SECURITYADMIN 及以上 | 是 | 是 |
| Database owner | 否 | 否 |
| Schema owner | 否 | **是** |
| Object owner | **是** | 否 |
| 有 MANAGE GRANTS 的角色 | 是 | 是 |

**易混对比：**
- **CURRENT_ROLE** 只看当前主角色；**IS_ROLE_IN_SESSION** 能判断继承来的角色
- **权限从下往上走**：`GRANT ROLE X TO ROLE Y` 读作"把 X 赋予 Y"，Y 是上层，拥有 X 的全部权限；X（下层）看不到 Y。`IS_ROLE_IN_SESSION` 是从当前角色往下找，上层能看到下层，下层看不到上层
- **Secondary Roles** 是会话里叠加多个角色；**把角色授给父角色** 是改层级，两者不同
- **MANAGE GRANTS** 能授权但不能建对象；建对象还要有对应的 CREATE 权限
- **账号角色** 全账号有效；**Database Role** 只在一个库内，且要通过账号角色激活

📝 本模块的题：[Q28](题库.md#q28)、[Q30](题库.md#q30)、[Q33](题库.md#q33)、[Q35](题库.md#q35)、[Q39](题库.md#q39)

#### 模块 2：认证（对应 44～48 讲）

| 概念 | 一句话定义 | 要点 |
|---|---|---|
| **Authentication Policy** | 控制客户端或用户**怎么认证**的策略 | 可设：允许的认证方式、客户端类型、MFA 要求、安全集成；默认全部允许；可设在账号或用户上，**用户级覆盖账号级**；`CLIENT_TYPES` 只是尽力而为，不能当唯一安全边界 |
| **MFA** | 密码之外再加一个验证因子，面向**使用密码的真人用户** | 支持：**Passkey、Authenticator App（TOTP）、Duo**；用户自己注册；管理员用 Authentication Policy 的 `MFA_ENROLLMENT = REQUIRED` 强制；服务账号不能用 MFA，要用别的认证方式 |
| **Federated Auth / SSO** | 由外部 IdP 做认证，Snowflake 是服务提供方（SP） | 支持 **SAML 2.0** 和 **OIDC**；Okta、Entra ID 有原生支持；SSO 用户默认不要求 MFA，依赖 IdP |
| **Key-pair Authentication** | 用 RSA 公私钥对代替密码认证，公钥存在用户上 | 最小 2048 位 RSA；私钥建议加密（推荐 AES-256-CBC），口令只保护本地私钥、不发给 Snowflake；适合脚本、自动化 |
| **密钥轮换** | 换新密钥而不中断连接 | 老方式：`RSA_PUBLIC_KEY` 和 `RSA_PUBLIC_KEY_2` 两个槽位，先设空的那个，客户端换新私钥后再清掉旧的；推荐的 named key pair 用 `ROTATE KEY PAIR`，旧钥默认再有效 24 小时 |
| **OAuth** | 客户端通过安全集成拿访问令牌 | **Snowflake OAuth**（`TYPE = OAUTH`，需要浏览器）；**External OAuth**（`TYPE = EXTERNAL_OAUTH`，适合程序化客户端） |
| **SCIM** | IdP 自动把用户和组（角色）同步到 Snowflake | 支持 Okta、Entra ID 和自定义 IdP；组对应角色；需用对应的 provisioner 角色拥有导入的用户和角色 |

**认证检查顺序：** 网络策略 → 认证策略 → 密码策略 → 会话策略。网络策略一旦拦截，后面不会再执行。

**易混对比：**
- **MFA** 面向真人；**Key-pair** 面向服务账号和自动化
- **SAML** 用 X.509 证书和 XML；**OIDC** 用 client ID/secret 和 JWT
- **Federated Auth（SSO）** 管"谁来认证"；**SCIM** 管"用户和角色从哪来"
- 为什么加密私钥：私钥文件泄露后，没有口令就不能直接冒用

📝 本模块的题：[Q32](题库.md#q32)、[Q40](题库.md#q40)、[Q106](题库.md#q106)、[Q108](题库.md#q108)

#### 模块 3：网络与加密（对应 49～50 讲）

| 概念 | 一句话定义 | 要点 |
|---|---|---|
| **Network Policy** | 按请求来源控制对 Snowflake 服务和 **internal stage** 的**入站**访问 | 默认任何地址都能连，直到应用策略；有 allowed 和 blocked 两个列表，**同一标识两边都有时 blocked 优先**；必须激活到账号、用户或安全集成才生效；**越具体越优先：安全集成 > 用户 > 账号** |
| **Network Rule** | 把网络标识分组的 schema 级对象，本身不表示允许或阻止 | 由使用它的功能决定允许还是阻止；`MODE`：`INGRESS`（入站）、`INTERNAL_STAGE`、`EGRESS`（出站，给外部网络访问用）；`TYPE`：`IPV4`、`IPV6`、`AWSVPCEID` 等 |
| **旧 IP 列表** | `ALLOWED_IP_LIST` / `BLOCKED_IP_LIST` | 仍可用，只支持 IPv4（IPv6 需用 network rule，`TYPE=IPV6`，仅 AWS、只管入站、不保护 internal stage）；network rule 更灵活；⚠️ 文档未写"应使用"，原清单的这句话没有核到原文 |
| **E2E Encryption** | 数据静态和传输中全程加密，第三方无法读取 | 传输用 TLS；internal stage 的文件在客户端上传前加密；external stage 推荐客户端加密 |
| **分层密钥模型** | 4 层：根密钥 → 账号主密钥 → 表主密钥 → 文件密钥 | 每个账号有独立密钥层级；AWS 和 Azure 的根密钥在 HSM 里 |
| **自动密钥轮换（rotation）** | 密钥使用超过 **30 天**自动换新 | 旧密钥只用来解密；加密新数据只用活动密钥 |
| **Periodic rekeying** | 退役密钥超过 **1 年**后，用新密钥重新加密数据，旧密钥不再使用 | 需要 **Enterprise 及以上**；在线完成，无停机；Fail-safe 里重加密的文件多收 7 天存储费 |
| **Tri-Secret Secure** | 客户在云 KMS 里管理的密钥，与 Snowflake 密钥组合成复合主密钥 | 客户可随时撤销密钥来切断访问；密钥不可用超过约 10 分钟，账号内所有数据操作停止；⚠️ 版本要求（Business Critical）该页没写，请到 Tri-Secret Secure 页面核对 |

**易混对比：**
- **Network Policy** 管"**从哪**来的请求"（入站）；**External Network Access** 管"**往哪**发请求"（出站，用 EGRESS 规则）
- **Network Policy / Rule** 管来源；**Authentication Policy** 管认证方式；**Tri-Secret** 管数据加密的密钥，三者不是一回事
- **Rotation**（30 天，旧钥保留用于解密）vs **Rekeying**（1 年，用新钥重新加密数据，旧钥不再使用）：别把 30 天和 1 年混了
- 想限制 internal stage 访问，需要把 network rule 用在 policy 里，并要求账号参数 `ENFORCE_NETWORK_RULES_FOR_INTERNAL_STAGES = TRUE`（AWS）

📝 本模块的题：[Q24](题库.md#q24)、[Q34](题库.md#q34)

#### 模块 4：数据保护与治理（对应 51～54、56 讲）

| 概念 | 一句话定义 | 要点 |
|---|---|---|
| **Masking Policy（动态数据脱敏）** | 在**查询时**按条件对**列**值脱敏，存储的数据不变 | **Enterprise 及以上**；schema 级对象，输入输出类型必须一致，一个策略可用于多个列；默认 object owner **不能**取消策略、也看不到被脱敏列的数据（职责分离） |
| **External Tokenization** | 数据加载前由第三方先令牌化，查询时通过外部函数还原 | 需要第三方令牌化服务；**不支持 Secure Data Sharing**（动态脱敏支持） |
| **Row Access Policy** | 决定查询能看到表或视图的**哪些行** | Enterprise 及以上；表达式返回 BOOLEAN；用**策略 owner 的角色**执行，所以用户不需要访问映射表；适用于 SELECT，以及 UPDATE、DELETE、MERGE 选中的行，**不阻止 INSERT** |
| **Secure View** | 带 `SECURE` 关键字的视图，用于数据隐私 | 仅授权用户（如 owner 角色）能看到定义；优化器不会重排 WHERE 之前的过滤，防止通过 UDF 泄露数据；**性能可能更慢**；标准视图和物化视图都能设为 secure；共享视图：默认只支持 secure view（"only secure views are supported in shares"，加标准视图会报错）；例外：创建 share 时设 `SECURE_OBJECTS_ONLY = FALSE` 才能共享非 secure 视图（设了就不能改回）；共享场景用 `CURRENT_ACCOUNT` 做按账号授权，`CURRENT_ROLE` / `CURRENT_USER` 在共享视图里返回 NULL（已对照官方 data-sharing 文档） |
| **Object Tag** | 键值对，值总是字符串，是 schema 级对象 | 一个对象最多 **50** 个标签；表上的标签被所有列继承；设置标签所有版本都可用，**标签传播和 tag-based masking 需要 Enterprise 及以上** |
| **Tag-based Masking** | 把 masking policy 绑到 tag 上，凡带该 tag 且类型匹配的列自动脱敏 | 直接绑在列上的策略**优先于**基于 tag 的策略 |
| **Classification** | 自动发现敏感列并打上系统标签 | Enterprise 及以上；两个系统标签：`SNOWFLAKE.CORE.SEMANTIC_CATEGORY`（语义类别，如姓名）和 `SNOWFLAKE.CORE.PRIVACY_CATEGORY`（`IDENTIFIER` / `QUASI_IDENTIFIER` / `SENSITIVE`）；可用 tag map 把系统标签映射成自定义标签 |

**易混对比：**
- 隐藏**列里的值** → Masking Policy；隐藏**整行** → Row Access Policy
- **一个列只能属于一种策略**：不能同时出现在 masking policy 和 row access policy 的参数里；两种策略都在一张表上时，**先执行 row access policy**
- **Tag** 负责"标记和找到"敏感数据；**Policy** 负责"真正保护"。Tag 本身不脱敏
- **Tag 的对象是元数据**（给表/视图/列分配标签、再查询标签来识别敏感数据，不碰实际数据）；**Tag-based masking** 的对象是**实际数据**（对带标签的列做脱敏）。题干问"识别/分配并查询元数据" → Tags，不是 Tag-based masking（Q23）
- **Masking Policy** vs **Secure View**：文档指出 secure view 没有职责分离，还会造成视图泛滥；masking 不用建一堆视图
- 条件里的角色函数：`CURRENT_ROLE` 看当前角色；`IS_ROLE_IN_SESSION` 会考虑角色层级；共享场景下 `CURRENT_ROLE`、`CURRENT_USER` 返回 NULL
- 策略都**不支持 future grants**，变通办法是把 `APPLY MASKING POLICY` / `APPLY ROW ACCESS POLICY` / `APPLY TAG` 授给自定义角色

📝 本模块的题：[Q23](题库.md#q23)、[Q27](题库.md#q27)、[Q29](题库.md#q29) ⚠️、[Q31](题库.md#q31)、[Q105](题库.md#q105) ⚠️

#### 模块 5：监控与脉络（对应 55、57、58 讲）

| 概念 | 一句话定义 | 要点 |
|---|---|---|
| **Account Usage** | `SNOWFLAKE` 共享库里的账号级历史和元数据视图 | 延迟：大多数视图约 2 小时，其余 45 分钟到 3 小时；历史视图保留 **1 年（365 天）**；**包含已删除对象**（名称为 NULL 表示已删除）；ACCOUNTADMIN 或有 `IMPORTED PRIVILEGES` 的角色可查 |
| **Information Schema** | 每个数据库自带的只读元数据 schema | **无延迟**；保留 7 天到 6 个月（视图或函数而定）；**不含已删除对象**；范围是单个数据库 |
| **ACCESS_HISTORY** | 记录查询读了哪些数据、语句写了哪些对象 | 在 `ACCOUNT_USAGE`；Enterprise 及以上；延迟约 3 小时，保留 1 年；一条语句一行；`direct_objects_accessed`（语句直接引用的对象和列）vs `base_objects_accessed`（底层源对象，含只用于过滤或 join 的列）；`objects_modified` 支撑**列级脉络**；`policies_referenced` 列出涉及的策略 |
| **Data Lineage（Snowsight）** | 在 Snowsight 里查看数据上下游和列级脉络 | Enterprise 及以上；保留 1 年；两种关系：数据移动（CTAS、INSERT、MERGE）和对象依赖（视图引用表）；需要 `VIEW LINEAGE` 权限（PUBLIC 默认有）加对象上的权限；不支持共享库、SNOWFLAKE 库和 INFORMATION_SCHEMA 里的对象 |
| **Alert** | schema 级对象：条件 + 满足时的动作 + 多久评估一次 | 创建后**默认挂起**，要 `ALTER ALERT ... RESUME`；省略 `WAREHOUSE` 即 serverless；创建需要账号级 `EXECUTE ALERT`（只有 ACCOUNTADMIN 能授予），serverless 还要 `EXECUTE MANAGED ALERT` |
| **Notification** | 把消息发到邮件、云厂商队列、Webhook | 邮件用 `SYSTEM$SEND_EMAIL`，其他用 `SYSTEM$SEND_SNOWFLAKE_NOTIFICATION` ⚠️（此项来自清单原有内容，本轮未重新核对） |

**易混对比：**
- **Account Usage** vs **Information Schema**：

| | Account Usage | Information Schema |
|---|---|---|
| 范围 | 整个账号 | 单个数据库 |
| 延迟 | 45 分钟到 3 小时 | 无 |
| 历史保留 | 1 年 | 7 天到 6 个月 |
| 已删除对象 | 包含 | 不含 |

- **Direct** vs **Base**：direct 是你写在 SQL 里的对象，base 是底层真正的数据来源
- **SHOW GRANTS** 列出"已授予角色/用户/share 的权限"（授权关系）；**谁实际读了数据**看 ACCESS_HISTORY（⚠️ SHOW GRANTS 页没有写"不记录访问"，这是两者用途不同的推论）
- **ACCESS_HISTORY**（SQL 查询，可做影响分析）vs **Data Lineage**（Snowsight 图形界面）：后者的列级脉络建立在前者的基础上
- **Alert**（按条件触发动作）vs **Notification**（只负责发消息）

📝 本模块的题：[Q22](题库.md#q22)、[Q25](题库.md#q25)

#### 其他：Git 集成（Q44）

题库答案和官方文档不一致，见 [Q44](题库.md#q44)，以文档为准（Git 集成 = Secret + API Integration）。这部分不在本章视频范围内。

### 按讲次排列的文档清单

### 访问控制（38～43 讲）
- [x] **38. Account Access & Security Introduction**（章节介绍，无需文档）

- [x] **39. Access Control Overview**
- ⭐⭐⭐ [Access Control Overview](https://docs.snowflake.com/en/user-guide/security-access-control-overview) — RBAC / DAC、securable object、owner、角色继承
- ⭐⭐ [Managed Access Schema](https://docs.snowflake.com/en/user-guide/security-access-control-configure#creating-managed-access-schemas) — 只有 schema owner / MANAGE GRANTS 能授权
- 📝 [Q30](题库.md#q30) Managed Access Schema

- [x] **40. Roles**
- ⭐⭐⭐ [System-defined roles](https://docs.snowflake.com/en/user-guide/security-access-control-overview#system-defined-roles) — ORGADMIN / ACCOUNTADMIN / SECURITYADMIN / USERADMIN / SYSADMIN / PUBLIC
- ⭐⭐ [Role hierarchy and privilege inheritance](https://docs.snowflake.com/en/user-guide/security-access-control-overview#role-hierarchy-and-privilege-inheritance)
- ⭐⭐ [Secondary roles (USE SECONDARY ROLES)](https://docs.snowflake.com/en/sql-reference/sql/use-secondary-roles)
- ⭐ [IS_ROLE_IN_SESSION](https://docs.snowflake.com/en/sql-reference/functions/is_role_in_session)
- 📝 [Q28](题库.md#q28) Secondary Roles
- 📝 [Q35](题库.md#q35) IS_ROLE_IN_SESSION
- 📝 [Q105](题库.md#q105) Masking Policy + 角色继承（角色继承出处；主归属在第 51 讲）
- 📝 [Q105 变体 1～4](题库.md#q105-variants)（自编练习：IS_ROLE_IN_SESSION、多角色继承与分支顺序、授权方向反了、Secondary Roles）
- 📝 [Q33](题库.md#q33) SYSADMIN 权限范围（还需 S5）

- [x] **41. Role Types: Database Roles [COF-C03]**
- ⭐⭐⭐ [Database roles](https://docs.snowflake.com/en/user-guide/security-access-control-overview#database-roles-and-role-hierarchies) — 限定在某个 database 内；用于 Data Sharing 授权；不能在会话中直接激活，需授给账号角色（账号角色不能授给 database role）；可授给 share 或其他 database role
- ⭐ [Account roles / Instance roles / Application roles](https://docs.snowflake.com/en/user-guide/security-access-control-overview#label-access-control-overview-role-types) — 其他角色类型

- [x] **42. Privileges**
- ⭐⭐⭐ [Access control privileges](https://docs.snowflake.com/en/user-guide/security-access-control-privileges) — USAGE / SELECT / OWNERSHIP / MANAGE GRANTS 等
- ⭐⭐ [Future Grants](https://docs.snowflake.com/en/user-guide/security-access-control-configure#defining-future-grants-on-database-or-schema-objects) — 对未来创建的对象预先授权( SQL文：GRANT SELECT ON FUTURE TABLES IN目標表 TO 目標ROLE )
- 📝 [Q39](题库.md#q39) Future Grants

- [ ] **43. Access Control - Hands-on**（实操：创建角色、授权、继承；复习 39～42 讲文档即可）

### 用户认证（44～48 讲）
- [x] **44. User Authentication**
- ⭐⭐⭐ [Authentication policies](https://docs.snowflake.com/en/user-guide/authentication-policies) — 限制认证方式、客户端类型、MFA 要求
- ⭐⭐ [Authentication overview](https://docs.snowflake.com/en/user-guide/admin-security-fed-auth-overview)

- [x] **45. Multi-factor Authentication (MFA)**
- ⭐⭐⭐ [MFA](https://docs.snowflake.com/en/user-guide/security-mfa) — 第二因子方式：Passkey（官方推荐，私钥）、Authenticator App (验证码TOTP Time-based One-Time Passcode）、Duo App；用户自行注册，管理员可用 Authentication Policy 限制允许的方式、强制注册；不支持邮件验证码/安全问题/社交登录
- 📝 [Q32](题库.md#q32) MFA 第二因子方式

- [ ] **46. Multi-factor Authentication (MFA) - Hands-on**（实操；MFA 文档同上，不用另读）

- [x] **47. Federated Authentication**
- ⭐⭐⭐ [Federated authentication & SSO](https://docs.snowflake.com/en/user-guide/admin-security-fed-auth-overview) — IdP + Snowflake 作为 SP；支持 SAML 2.0 和 OpenID Connect（OIDC）；部分厂商（如 Okta、ADFS）有原生支持
- ⭐ [Configuring Snowflake to use federated authentication](https://docs.snowflake.com/en/user-guide/admin-security-fed-auth-use)

- [ ] **48. Key Pair Authentication, OAuth & SCIM**
- ⭐⭐⭐ [Key-pair authentication](https://docs.snowflake.com/en/user-guide/key-pair-auth) 用户名密码的替代方案；密钥轮换replacement of active keys；RSA_PUBLIC_KEY / RSA_PUBLIC_KEY_2 轮换；私钥可加密；适合服务账号
- ⭐⭐ [OAuth](https://docs.snowflake.com/en/user-guide/oauth) — Snowflake OAuth / External OAuth
- ⭐⭐ [SCIM](https://docs.snowflake.com/en/user-guide/scim-intro) — 自动同步用户和角色（Okta / Azure AD）
- 📝 [Q40](题库.md#q40) Key-pair 密钥轮换
- 📝 [Q106](题库.md#q106) 私钥加密
- 📝 [Q108](题库.md#q108) 服务账号用 Key-pair

### 网络与加密（49～50 讲）
- [x] **49. Network Policies**
- ⭐⭐⭐ [Network policies](https://docs.snowflake.com/en/user-guide/network-policies) — 控制谁能访问 Snowflake 服务和 internal stage；allowed 列表 / blocked 列表，同一地址两边都有时 blocked 优先；可应用到账号、security integration、用户，越具体的优先（**integration > 用户 > 账号**，具体的覆盖宽泛的，不合并）；network rule 比旧的 ALLOWED_IP_LIST 更灵活
- ⭐⭐ [Network rules](https://docs.snowflake.com/en/user-guide/network-rules) — 把相关网络标识（IPv4/IPv6、AWS VPCE 等）分组，加入 network policy 的 allowed/blocked 列表（也用于出站网络访问）
- 📝 [Q34](题库.md#q34) Network Rules / Network Policies

- [x] **50. Data Encryption**（已勾选）
- ⭐⭐⭐ [End-to-end encryption](https://docs.snowflake.com/en/user-guide/security-encryption-end-to-end) — 数据存储和传输全程加密（AES-256）
- ⭐⭐⭐ [Encryption key management](https://docs.snowflake.com/en/user-guide/security-encryption-manage) — 分层密钥、密钥超过 30 天自动轮换（rotation）；Enterprise 起可开启 periodic rekeying（退役密钥超过 1 年后重新加密数据）、Tri-Secret Secure（客户管理密钥 + Snowflake 密钥，需 Business Critical）  
- ⭐⭐⭐ [Tri-Secret Secure overview](https://docs.snowflake.com/en/user-guide/security-encryption-tss#label-encryption-tss-overview) — 客户管理密钥（CMK，存于云 KMS）与 Snowflake 管理密钥组合成复合主密钥；撤销客户密钥即可使数据不可访问；需 Business Critical 及以上
- 📝 [Q24](题库.md#q24) Tri-Secret Secure（还需 S2-11）

### 数据治理与访问控制（51～54 讲）
- [ ] **51. Column Level Security**
- ⭐⭐⭐ [Dynamic Data Masking / Masking policies](https://docs.snowflake.com/en/user-guide/security-column-intro) — 查询时按角色脱敏（数据本身不变）；External Tokenization；需 Enterprise 及以上
- 📝 [Q31](题库.md#q31) Masking Policy
- 📝 [Q105](题库.md#q105) Masking Policy + 角色继承 ⚠️（还需第 40 讲 Roles 的 Role hierarchy and privilege inheritance，第 39 讲 Overview 也有提及）
- 📝 [Q105 变体 1～4](题库.md#q105-variants)（自编练习：IS_ROLE_IN_SESSION、多角色继承与分支顺序、授权方向反了、Secondary Roles）

- [ ] **52. Row Level Security**
- ⭐⭐⭐ [Row access policies](https://docs.snowflake.com/en/user-guide/security-row-intro) — 按角色/映射表决定能看到哪些行allowed to see a particular record in a table；需 Enterprise 及以上
- 📝 [Q27](题库.md#q27) Row Access Policy

- [ ] **53. Column and Row Level Security - Hands-on**（实操；文档同 51、52 讲）

- [ ] **54. Secure Views**
- ⭐⭐⭐ [Secure views](https://docs.snowflake.com/en/user-guide/views-secure) — 隐藏视图定义（仅 owner 角色可见），防止优化器泄露数据（代价是性能略低）；Data Sharing 必须用 secure view

### 监控与脉络（55～58 讲）
- [ ] **55. Account Usage & Information Schema**
- ⭐⭐⭐ [Account Usage](https://docs.snowflake.com/en/sql-reference/account-usage) — SNOWFLAKE 共享库；有延迟（最长约 3 小时），保留 365 天
- ⭐⭐⭐ [Information Schema](https://docs.snowflake.com/en/sql-reference/info-schema) — 每个库自带；实时，保留较短（约 7 天～6 个月，视视图而定）
- ⭐⭐ [Access History](https://docs.snowflake.com/en/user-guide/access-history) — 记录谁读/写了哪些列 identify which users, dashboards, reports and processes might be affected，在 ACCOUNT_USAGE 的 ACCESS_HISTORY 视图；延迟约 3 小时，保留 1 年；Enterprise 及以上
- 📝 [Q22](题库.md#q22) ACCESS_HISTORY 影响分析（58 讲脉络也相关）

- [x] **56. Object Tagging Hands-on**
- ⭐⭐⭐ [Object tagging](https://docs.snowflake.com/en/user-guide/object-tagging) — 标签继承（上层对象的标签自动继承给下层，如表→列）；创建/设置标签所有版本可用，tag-based masking 等进阶功能需 Enterprise 及以上
- ⭐⭐ [Classification](https://docs.snowflake.com/en/user-guide/governance-classify-concepts) — 自动识别敏感列
- 📝 [Q23](题库.md#q23) Object Tagging 识别敏感数据
- 📝 [Q29](题库.md#q29) 审计敏感数据访问 ⚠️（答案 D=Object tagging；与 51 讲 tag-based masking 相关）

- [ ] **57. Alerts & Notifications [COF-C03]**
- ⭐⭐⭐ [Alerts](https://docs.snowflake.com/en/user-guide/alerts) — 按计划检查条件，满足时触发动作；使用 serverless 或指定 warehouse；创建后默认挂起，需 ALTER ALERT ... RESUME
- ⭐⭐ [Notifications / Notification integrations](https://docs.snowflake.com/en/user-guide/notifications/about-notifications) — 可发到云厂商队列（SNS / Pub/Sub / Event Grid）、邮件、Webhook；邮件用 SYSTEM$SEND_EMAIL，其他用 SYSTEM$SEND_SNOWFLAKE_NOTIFICATION

- [ ] **58. Data Lineage - Hands-on [COF-C03]**
- ⭐⭐⭐ [Data lineage](https://docs.snowflake.com/en/user-guide/ui-snowsight-lineage) — Snowsight 中查看上下游（upstream/downstream）依赖，含列级脉络（Column lineage）；需 Enterprise 及以上
- 📝 [Q25](题库.md#q25) 列级脉络 ACCESS_HISTORY（答案 ACCESS_HISTORY，55 讲已讲过）
- ⭐ [Aggregation Policies](https://docs.snowflake.com/en/user-guide/aggregation-policies) / [Projection Policies](https://docs.snowflake.com/en/user-guide/projection-policies) — 课程未单列，了解即可

## セクション5: Performance Concepts: Virtual Warehouses

按课程每一讲对应官方文档（59～69 讲，共 11 讲；章节标题写 12 项，多出的 1 项应是测验之类）。题目按讲次归属为推测，以题库内容为准。

### 📌 学习顺序（先文档 → 做题 → 错了再看视频）

本章内容少、考点集中（9 题，都是看场景选配置），文档短，适合先读文档再做题。

1. **66 Multi-cluster**（Q68 / Q79 / Q83）：Scale up vs scale out、Maximized vs Auto-scale、Standard vs Economy
2. **69 QAS**（Q63 / Q88）：适合哪类查询
3. **60～62 概览 / 大小 / 属性**（61：Q81；62：Q82 还需 S6）
4. **64～65 Resource Monitors / 负载监控**（Q110 / Q71；学完可做 S2-31 的 Q37 / Q38）
5. **67 / 68** 题库无题，但是 C03 新增，各扫一遍文档
6. **可跳过**：59（简介）、63（实操）；65 只看 Resource Monitor 创建部分
7. 学完把 6 组对比写成小表：Scale up/out、Maximized/Auto-scale、Standard/Economy、QAS 适用查询、排队判断、Resource Monitor 动作

### 基础（59～63 讲）
- [ ] **59. Performance Concepts: Virtual Warehouses Introduction**（章节介绍，无需文档）

- [ ] **60. Virtual Warehouse Overview**
- ⭐⭐⭐ [Overview of Warehouses](https://docs.snowflake.com/en/user-guide/warehouses-overview) — 虚拟仓库 = 计算层；尺寸、auto-suspend / auto-resume

- [ ] **61. Virtual Warehouse Sizing and Billing**
- ⭐⭐⭐ [Warehouse sizes & credit usage](https://docs.snowflake.com/en/user-guide/warehouses-overview#warehouse-size) — 每升一档 credit 翻倍；按秒计费，每次启动最少 60 秒
- ⭐⭐ [Warehouse Considerations](https://docs.snowflake.com/en/user-guide/warehouses-considerations) — Scale up（变大，提升单条复杂查询）vs Scale out（多集群，提升并发）
  - 性质相近（homogeneous）的查询放同一仓库，负载不同就用不同仓库（加载 vs 报表 → Q81）
  - 仓库缓存在挂起（suspend）时被清空，所以挂起后第一条查询会变慢；AUTO_SUSPEND 建议设得低（5～10 分钟以内），但要配合查询间隔（Q82 / Q84）
- ⭐⭐ [Queries too large to fit in memory](https://docs.snowflake.com/en/user-guide/performance-query-warehouse-memory) — Spilling 解法：加大仓库 / 分小批处理（Q70）
- ⭐⭐ [Understanding compute cost](https://docs.snowflake.com/en/user-guide/cost-understanding-compute)
- 📝 [Q70](题库.md#q70) 减少内存溢出 Spilling（S6，用「加大仓库」解；还需 S6）
- 📝 [Q81](题库.md#q81) 按负载拆分仓库

- [ ] **62. Virtual Warehouse State & Properties**
- ⭐⭐⭐ [Working with Warehouses](https://docs.snowflake.com/en/user-guide/warehouses-tasks) — Started / Suspended / Resizing；AUTO_SUSPEND、AUTO_RESUME、STATEMENT_TIMEOUT 等属性
- ⭐⭐ [CREATE WAREHOUSE](https://docs.snowflake.com/en/sql-reference/sql/create-warehouse) — 以下属性的定义和默认值在这页（Working with Warehouses 页不列属性）
  - **状态**：Started（运行中，计费）/ Suspended（已暂停，不计费）/ Resuming（启动中）；运行中调整大小**不影响正在执行的语句**，新资源就绪后用于排队中和之后的语句
  - **AUTO_SUSPEND**：空闲多少秒后自动暂停（默认 600 秒）；设 0 或 NULL = 永不暂停（官方不推荐）。暂停会清空仓库本地缓存，太短会丢缓存、频繁重启（每次启动最少按 60 秒计费）（Q82）
  - **AUTO_RESUME**：有查询提交时是否自动启动（默认 TRUE）；FALSE 时必须手动 `ALTER WAREHOUSE ... RESUME`
  - **STATEMENT_TIMEOUT_IN_SECONDS**：单条语句最长运行时间，超时自动取消（默认 172800 秒 = 2 天）；可在账户 / 用户 / 会话 / 仓库级设置，仓库和会话都设时取**较小的非零值**
  - **STATEMENT_QUEUED_TIMEOUT_IN_SECONDS**：语句最长排队等待时间，超时取消（默认 0 = 不限制）
  - **INITIALLY_SUSPENDED**：创建后是否直接处于暂停状态（默认 FALSE，即创建后立即启动）
- 📝 [Q82](题库.md#q82) AUTO_SUSPEND 与仓库缓存（还需 S6）
- 📝 [Q84](题库.md#q84) 仓库缓存（挂起后丢失）（S6，还需本讲）

- [ ] **63. Virtual Warehouse Overview - Hands-on**（实操：创建、调整仓库；复习 60～62 讲文档即可）

### 监控（64～65 讲）
- [ ] **64. Resource Monitors**
- ⭐⭐⭐ [Resource monitors](https://docs.snowflake.com/en/user-guide/resource-monitors) — 设 credit 配额，达到阈值时通知 / 挂起；账号级或仓库级
- 📝 [Q37](题库.md#q37) Budgets（S2-31，学完本讲可做）
- 📝 [Q38](题库.md#q38) Budget vs Resource Monitor（S2-31，学完本讲可做）
- ⭐⭐ [Budgets](https://docs.snowflake.com/en/user-guide/budgets) — 对比 Resource Monitor 用

  **Resource Monitor vs Budget（Account / Custom）**

  | 维度 | Resource Monitor | Budget（Account / Custom） |
  |---|---|---|
  | **监控对象** | **只管用户自建的 warehouse**（user-managed warehouses） | Account budget 管整个账号；**Custom budget 管你指定的一组对象**，可以是 warehouse、database、schema、table、pipe、task、MV、compute pool 等，**也能按 object tag 归组** |
  | **Serverless 功能** | ❌ **不能追踪** Snowpipe、Auto-clustering、MV 维护、Serverless task、Search Optimization 等 | ✅ **包含 serverless 功能的花费** |
  | **作用级别** | 账号级（一个账号最多一个）或仓库级（一个 RM 可以管多个仓库，但一个仓库只能挂一个 RM） | 账号级一个 account budget，另外可以建多个 custom budget |
  | **周期** | 可以选 Daily / Weekly / Monthly / Yearly / Never，也能自定义起止时间 | **只有月度**，每月 1 日 00:00 UTC 重置 |
  | **额度单位** | Credit quota | 每月 spending limit（以 credit 计） |
  | **触发方式** | 按**实际已用量**达到阈值触发（例如用到 80%、100%） | 按**预测值（projected/forecast）**判断，预计会超支时提前通知 |
  | **能做的动作** | Notify；Notify & Suspend（等正在跑的查询完成后挂起）；Notify & Suspend Immediately（取消正在跑的查询并挂起）。**能直接挂起仓库** | 主要是**通知**：邮件、云消息队列（SNS / Event Grid / Pub/Sub）、Webhook（Slack / Teams 等）；也可以触发存储过程做自定义动作 |
  | **谁能创建** | 只有 **ACCOUNTADMIN**（可以把 MONITOR / MODIFY 权限授给其他角色） | Custom budget 可以由拥有相应权限的非 ACCOUNTADMIN 角色创建 |
  | **自身成本** | 不额外收费 | 本身会消耗少量 serverless credit |

  - 记法：**RM 是"刹车"**（只管 warehouse，按实际用量到阈值后能直接挂起仓库）；**Budget 是"预警仪表盘"**（覆盖范围更广，含 serverless、可以按 tag 归组，按月度预测提前通知）
  - 题眼：**Snowpipe / serverless / tag / forecast / projected → Budgets**；**suspend warehouse / credit quota / 阈值后挂起 → Resource Monitor**

- [ ] **65. Compute Monitoring & Resource Monitors - Hands-on**
- ⭐⭐ [Monitoring warehouse load](https://docs.snowflake.com/en/user-guide/warehouses-load-monitoring) — 查看仓库负载、排队；**有查询在排队（Queued）= 仓库过载**（Q71）
  - 对策：负载高或有排队 → 另开仓库分流，或调整多集群设置加集群；负载低但慢 → 加大仓库
- ⭐⭐ [QUERY_HISTORY view](https://docs.snowflake.com/en/sql-reference/account-usage/query_history) — `QUEUED_OVERLOAD_TIME`：因仓库负载（并发）排队的时间
- 📝 [Q110](题库.md#q110) QUEUED_OVERLOAD_TIME
- 📝 [Q71](题库.md#q71) 判断仓库过载（排队）

### 扩展与新仓库类型（66～68 讲）
- [x] **66. Multi-cluster Warehouses**
- ⭐⭐⭐ [Multi-cluster Warehouses](https://docs.snowflake.com/en/user-guide/warehouses-multicluster) — 需 Enterprise 及以上

  | 项目 | 选项 | 说明 |
  |---|---|---|
  | **集群数量参数** | `MIN_CLUSTER_COUNT` / `MAX_CLUSTER_COUNT` | 设置集群数量的下限和上限 |
  | **模式** | Maximized Mode | MIN = MAX，所有集群一直运行 |
  | | Auto-scale Mode | MIN < MAX，根据负载自动增减集群 |
  | **扩展策略 Scaling Policy**（`SCALING_POLICY`，只在 Auto-scale 模式下生效） | Standard scaling policy（默认） | 一出现排队就加集群，优先避免排队 |
  | | Economy scaling policy | 预计新集群能忙满 6 分钟才启动；剩余工作不足 6 分钟就关闭。省 credit，但可能排队 |

  **速记：看场景选配置**（Q79）
  - 并发高、有高峰低谷 → Auto-scale mode + Standard scaling policy
  - 省 credit、可以接受一些排队 → Auto-scale mode + Economy scaling policy
  - 并发高而且一直稳定（MIN = MAX）→ Maximized Mode
  - 单个查询慢、数据加载 → Scale up（加大仓库规格）

- 📝 [Q68](题库.md#q68) Economy 扩展策略
- 📝 [Q79](题库.md#q79) 多集群 Auto-scale + Standard
- 📝 [Q83](题库.md#q83) MIN/MAX_CLUSTER_COUNT

- [x] **67. Warehouse Generation [COF-C03]**
- ⭐⭐ [Gen2 standard warehouses](https://docs.snowflake.com/en/user-guide/warehouses-gen2) — Gen2 是标准仓库的升级版（更快的硬件 + DELETE/UPDATE/MERGE、表扫描优化）；`GENERATION = '1'/'2'` 设置，不写默认 Gen2；Gen1 ⇄ Gen2 可用 ALTER WAREHOUSE 互转；仅标准仓库，不适用 Snowpark-optimized；新建 Gen2 默认开启 QAS（Gen1 改成 Gen2 不会自动开）；Snowsight 暂不支持，需用 SQL

- [ ] **68. Snowpark Optimized Warehouses [COF-C03]**
- ⭐⭐ [Snowpark-optimized warehouses](https://docs.snowflake.com/en/user-guide/warehouses-snowpark-optimized) — 单节点内存更大（默认是标准仓库的 16 倍），适合 ML 训练等内存密集型 Snowpark 工作负载，或依赖特定 CPU 架构（x86）的负载
  - `RESOURCE_CONSTRAINT`：MEMORY_1X（16GB，最小 XS）/ **MEMORY_16X（256GB，默认，最小 M）** / MEMORY_64X（1TB，最小 L，仅 AWS 预览）；每种都有 `_x86` 版本
  - 创建和恢复比标准仓库慢

### 加速（69 讲）
- [ ] **69. Query Acceleration Service (QAS)**
- ⭐⭐⭐ [Query Acceleration Service](https://docs.snowflake.com/en/user-guide/query-acceleration-service) — 给单个很耗资源的查询临时加算力。把部分扫描、过滤卸载到 serverless 计算，减轻离群查询（outlier）影响
  - 适合：大扫描 + 选择性过滤/聚合；大扫描 + 插入/复制大量新行（INSERT、COPY）（Q63）
  - 新建的 Gen2 标准仓库、多集群仓库默认开启 QAS
  - 常见不适用：分区（partitions）太少、过滤选择性不够、GROUP BY 基数过高、阻止加速的 LIMIT 子句（LIMIT clause，系统自动判断）、SEQ/RANDOM 等非确定性函数
  - `ENABLE_QUERY_ACCELERATION = TRUE`；`QUERY_ACCELERATION_MAX_SCALE_FACTOR`：仓库大小倍数，默认 8，0 = 无上限
  - Enterprise+；按秒计 serverless credit，与仓库分开计费
  - 判断适用：`SYSTEM$ESTIMATE_QUERY_ACCELERATION`、`QUERY_ACCELERATION_ELIGIBLE` 视图
- 📝 [Q63](题库.md#q63) Query Acceleration Service 适用查询
- 📝 [Q88](题库.md#q88) Query Acceleration Service

### 其他
- 📝 [Q33](题库.md#q33) SYSADMIN 权限范围（S4，还需 S5：SYSADMIN 创建仓库）
- 📝 [Q84](题库.md#q84) 仓库缓存（S6，还需 S5）

## セクション6: Performance Concepts: Query Optimisation

- ⭐⭐⭐ [Performance Optimization 总览](https://docs.snowflake.com/en/guides-overview-performance)
- ⭐⭐⭐ [Micro-partitions & Data Clustering](https://docs.snowflake.com/en/user-guide/tables-clustering-micropartitions) — Pruning、clustering depth
- ⭐⭐⭐ [Query Profile](https://docs.snowflake.com/en/user-guide/ui-query-profile) — Spilling、Exploding joins
- ⭐⭐⭐ [Persisted Query Results](https://docs.snowflake.com/en/user-guide/querying-persisted-results) — 结果缓存 24 小时
- ⭐⭐ [Clustering Keys](https://docs.snowflake.com/en/user-guide/tables-clustering-keys)
- ⭐⭐ [Search Optimization Service](https://docs.snowflake.com/en/user-guide/search-optimization-service)
- ⭐⭐ [Materialized Views](https://docs.snowflake.com/en/user-guide/views-materialized)
- ⭐ [Queries 总览](https://docs.snowflake.com/en/guides-overview-queries)

### 📝 本章题目（题库）

| 讲次 | 题号 | 知识点 | 还需 |
|---|---|---|---|
| S6 | [Q36](题库.md#q36) | ACCOUNT_USAGE 延迟 |  |
| S6 | [Q61](题库.md#q61) | QUERY_INSIGHTS |  |
| S6 | [Q64](题库.md#q64) | MV / Dynamic Table 加速聚合 | S2-15, S2-26 |
| S6 | [Q65](题库.md#q65) | Query Profile |  |
| S6 | [Q66](题库.md#q66) | Automatic Clustering |  |
| S6 | [Q69](题库.md#q69) | Query Profile 看剪枝 |  |
| S6 | [Q70](题库.md#q70) | 减少内存溢出 Spilling | S5 |
| S6 | [Q72](题库.md#q72) | Search Optimization Service |  |
| S6 | [Q73](题库.md#q73) | Exploding Join |  |
| S6 | [Q76](题库.md#q76) | Materialized View 适用场景 | S2-15 |
| S6 | [Q80](题库.md#q80) | Clustering Depth |  |
| S6 | [Q84](题库.md#q84) | 仓库缓存（挂起后丢失） | S5 |
| S6 | [Q87](题库.md#q87) | 三种缓存 |  |
| S6 | [Q89](题库.md#q89) | Metadata Cache（COUNT(*)） |  |
| S6 | [Q90](题库.md#q90) | 笛卡尔积 Join |  |
| S6 | [Q92](题库.md#q92) | Clustering 受益场景 |  |
| S6 | [Q93](题库.md#q93) | SOS Search Access Path |  |
| S6 | [Q109](题库.md#q109) | Result Cache 失效时机 |  |
| S6 | [Q111](题库.md#q111) | 仓库缓存 |  |

## セクション7: Data Loading & Unloading

- ⭐⭐⭐ [Data Loading 总览](https://docs.snowflake.com/en/guides-overview-loading-data) — Stage 类型（User / Table / Named / External）
- ⭐⭐⭐ [Bulk Loading from Local File System](https://docs.snowflake.com/en/user-guide/data-load-local-file-system) — PUT、COPY INTO、文件格式
- ⭐⭐⭐ [Snowpipe](https://docs.snowflake.com/en/user-guide/data-load-snowpipe-intro) — 自动加载、serverless
- ⭐⭐ [Data Loading Considerations](https://docs.snowflake.com/en/user-guide/data-load-considerations) — 推荐文件大小 100～250MB（压缩后）
- ⭐⭐ [Data Unloading](https://docs.snowflake.com/en/guides-overview-unloading-data) — COPY INTO &lt;location&gt;、GET
- ⭐ [Directory Tables](https://docs.snowflake.com/en/user-guide/data-load-dirtables)
- ⭐ [Connectors（Kafka 等）](https://other-docs.snowflake.com/en/connectors)

### 📝 本章题目（题库）

| 讲次 | 题号 | 知识点 | 还需 |
|---|---|---|---|
| S7 | [Q41](题库.md#q41) | VALIDATE 函数 |  |
| S7 | [Q42](题库.md#q42) | Snowpipe Streaming |  |
| S7 | [Q43](题库.md#q43) | COPY INTO 的 PATTERN |  |
| S7 | [Q45](题库.md#q45) | 大文件拆分加载 |  |
| S7 | [Q46](题库.md#q46) | COPY 加载非结构化数据 ⚠️ |  |
| S7 | [Q47](题库.md#q47) | Load Metadata 跳过已加载文件 |  |
| S7 | [Q48](题库.md#q48) | FILES 参数最快 |  |
| S7 | [Q49](题库.md#q49) | 卸载 Parquet 带表头 |  |
| S7 | [Q50](题库.md#q50) | Directory Table |  |
| S7 | [Q51](题库.md#q51) | SINGLE = TRUE 卸载 |  |
| S7 | [Q52](题库.md#q52) | 卸载在 Query History 中的关键字 ⚠️ |  |
| S7 | [Q53](题库.md#q53) | NULL_IF |  |
| S7 | [Q54](题库.md#q54) | Auto-ingest Snowpipe |  |
| S7 | [Q57](题库.md#q57) | 卸载 Parquet 指定列类型 |  |
| S7 | [Q58](题库.md#q58) | Directory Table 用途 |  |
| S7 | [Q59](题库.md#q59) | 命名 File Format |  |

## セクション8: Data Transformations

- ⭐⭐⭐ [Semi-structured Data](https://docs.snowflake.com/en/user-guide/semistructured-intro) — VARIANT、FLATTEN、`:` 路径写法
- ⭐⭐⭐ [Streams](https://docs.snowflake.com/en/user-guide/streams-intro) — CDC
- ⭐⭐⭐ [Tasks](https://docs.snowflake.com/en/user-guide/tasks-intro) — 任务 DAG
- ⭐⭐ [Dynamic Tables](https://docs.snowflake.com/en/user-guide/dynamic-tables/overview)
- ⭐⭐ [UDF](https://docs.snowflake.com/en/developer-guide/udf/udf-overview) — 支持的语言
- ⭐⭐ [Stored Procedures](https://docs.snowflake.com/en/developer-guide/stored-procedure/stored-procedures-overview) — 和 UDF 的区别
- ⭐ [Unstructured Data](https://docs.snowflake.com/en/user-guide/unstructured-intro)
- ⭐ [Data Types](https://docs.snowflake.com/en/sql-reference/intro-summary-data-types)

### 📝 本章题目（题库）

| 讲次 | 题号 | 知识点 | 还需 |
|---|---|---|---|
| S8 | [Q74](题库.md#q74) | VARIANT 中影响性能的值 ⚠️ |  |
| S8 | [Q75](题库.md#q75) | GET_PATH |  |
| S8 | [Q77](题库.md#q77) | TRY_TO_DATE |  |
| S8 | [Q78](题库.md#q78) | OBJECT_CONSTRUCT |  |

## セクション9: Storage, Data Protection & Data Sharing

- ⭐⭐⭐ [Time Travel](https://docs.snowflake.com/en/user-guide/data-time-travel) — 0～90 天、UNDROP、AT / BEFORE
- ⭐⭐⭐ [Fail-safe](https://docs.snowflake.com/en/user-guide/data-failsafe) — 7 天，仅 Snowflake 可恢复
- ⭐⭐⭐ [Table Storage Considerations](https://docs.snowflake.com/en/user-guide/tables-storage-considerations) — Permanent / Transient / Temporary
- ⭐⭐⭐ [Cloning Considerations](https://docs.snowflake.com/en/user-guide/object-clone) — Zero-copy clone
- ⭐⭐⭐ [Secure Data Sharing](https://docs.snowflake.com/en/user-guide/data-sharing-intro) — Share、Reader Account
- ⭐⭐ [Collaboration 总览](https://docs.snowflake.com/en/guides-overview-sharing)
- ⭐⭐ [What are listings?](https://other-docs.snowflake.com/en/collaboration/collaboration-listings-about) — Marketplace
- ⭐⭐ [Replication & Failover](https://docs.snowflake.com/en/user-guide/account-replication-intro)
- ⭐ [Client Redirect](https://docs.snowflake.com/en/user-guide/client-redirect)

### 📝 本章题目（题库）

| 讲次 | 题号 | 知识点 | 还需 |
|---|---|---|---|
| S9 | [Q5](题库.md#q5) | 哪些对象可 UNDROP | S2-14 |
| S9 | [Q10](题库.md#q10) | Zero-copy Clone 后的存储 |  |
| S9 | [Q20](题库.md#q20) | 跨云 Failover Group |  |
| S9 | [Q26](题库.md#q26) | TABLE_STORAGE_METRICS |  |
| S9 | [Q86](题库.md#q86) | 克隆的 Task 初始状态 | S2-25 |
| S9 | [Q94](题库.md#q94) | SECURE_OBJECTS_ONLY ⚠️ |  |
| S9 | [Q95](题库.md#q95) | Marketplace 消费者 |  |
| S9 | [Q96](题库.md#q96) | Public Listing ⚠️ |  |
| S9 | [Q97](题库.md#q97) | Secure View 共享 | S2-15 |
| S9 | [Q98](题库.md#q98) | 跨数据库 Share |  |
| S9 | [Q99](题库.md#q99) | Listing 消费者能做什么 |  |
| S9 | [Q100](题库.md#q100) | 同区域账号间共享 |  |
| S9 | [Q104](题库.md#q104) | Replication & Failover Groups |  |
| S9 | [Q113](题库.md#q113) | REFERENCE_USAGE |  |
| S9 | [Q114](题库.md#q114) | Limited Trial Listing |  |
| S9 | [Q115](题库.md#q115) | Private Listing 发布前步骤 |  |
| S9 | [Q116](题库.md#q116) | 共享数据的消费者成本 | S2-31 |

⚠️ = 给出的答案与社区投票不一致或有争议，详见题库中的说明。
