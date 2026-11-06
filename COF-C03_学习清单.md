# COF-C03 (SnowPro Core) 官方文档学习清单

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
- ⭐⭐⭐ [Micro-partitions & Data Clustering](https://docs.snowflake.com/en/user-guide/tables-micro-partitions) — 列式存储、微分区 50～500MB（未压缩）
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
| `TABLE` | Permanent（永久表） | 0～90 天 | 7 天 |
| `TRANSIENT` | Transient（跨会话临时表） | 0～1 天 | 无 |
| `TEMPORARY` | Temporary（仅当前会话） | 0～1 天 | 无 |

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
- ⭐ [Worksheets](https://docs.snowflake.com/en/user-guide/ui-snowsight-worksheets) — 计划弃用
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
- ⭐⭐⭐ [Parameters](https://docs.snowflake.com/en/sql-reference/parameters) — Account / Session / Object 三类，层级覆盖关系（覆盖规则：越靠近使用者优先级越高，即 Account → User → Session → Object，后者覆盖前者）
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
  - Append-only：建在标准表上，只记录新增行，不记录删除
  - 地理空间数据（geospatial）：Standard stream 取不到变更数据，建议用 append-only stream
  - Insert-only：建在 external tables、externally managed Iceberg tables 上，只记录新增
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
    - `REFRESH_MODE`：
      - `AUTO`（默认）：Snowflake 根据查询自动选 INCREMENTAL 或 FULL；不支持增量的查询会退回 FULL。
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
- [ ] **31. Billing Overview**
- ⭐⭐⭐ [Understanding Overall Cost](https://docs.snowflake.com/en/user-guide/cost-understanding-overall) — 计算 / 存储 / 数据传输；Cloud Services 超过当日计算 10% 才收费
- ⭐⭐ [Cost & Billing](https://docs.snowflake.com/en/guides-overview-cost)
- 📝 [Q37](题库.md#q37) Budgets（还需 S5）
- 📝 [Q38](题库.md#q38) Budget vs Resource Monitor（还需 S5）

## セクション3: Snowflake AI Data Cloud Features and Architecture (AI Features)

按课程每一讲对应官方文档（32～37 讲）。

### 总览
- ⭐⭐⭐ [Snowflake AI & ML](https://docs.snowflake.com/en/guides-overview-ai-features) — Cortex AI Functions、Cortex Search、Cortex Analyst、Document AI、ML Functions
- ⭐ [Snowflake Horizon Catalog](https://docs.snowflake.com/en/user-guide/snowflake-horizon)

### Snowpark 与 Notebooks（32～34 讲）
- [ ] **32. Introduction to Snowpark**
- ⭐⭐⭐ [Snowpark API](https://docs.snowflake.com/en/developer-guide/snowpark/index) — 支持 Python / Java / Scala；DataFrame 惰性执行，在 Snowflake 内下推计算
- 📝 [Q15](题库.md#q15) Snowpark 支持语言（S2-29，学完本讲更稳）
- 📝 [Q21](题库.md#q21) 用 Python 连接 Snowflake（S2-29，学完本讲更稳）

- [ ] **33. Introduction to Snowflake Notebooks [COF-C03]**
- ⭐⭐⭐ [Snowflake Notebooks](https://docs.snowflake.com/en/user-guide/ui-snowsight/notebooks) — Snowsight 内的 SQL / Python 交互式开发环境

- [ ] **34. Executing SQL and Python Snowpark in Notebooks [COF-C03]**
- ⭐⭐ [Develop and run code in Snowflake Notebooks](https://docs.snowflake.com/en/user-guide/ui-snowsight/notebooks-develop-run) — SQL 单元格、Python 单元格、Snowpark Session

### Cortex AI（35～36 讲）
- [ ] **35. Snowflake Cortex: AISQL, Copilot & Document AI [COF-C03]**
- ⭐⭐⭐ [Cortex AI Functions (AISQL)](https://docs.snowflake.com/en/user-guide/snowflake-cortex/aisql) — COMPLETE / SUMMARIZE / SENTIMENT / TRANSLATE / AI_EXTRACT 等，直接在 SQL 里调 LLM，无需自建模型
- ⭐⭐ [Snowflake Copilot](https://docs.snowflake.com/en/user-guide/snowflake-copilot) — 自然语言辅助写 SQL
- ⭐⭐ [Document AI](https://docs.snowflake.com/en/user-guide/snowflake-cortex/document-ai/overview) — 从文档提取结构化信息
- ⭐⭐ [ML Functions](https://docs.snowflake.com/en/guides-overview-ml-functions) — Forecasting、Anomaly Detection、Top Insights（归到本讲为推测）
- 📝 [Q14](题库.md#q14) ML Functions（Top Insights）
- 📝 [Q102](题库.md#q102) Snowflake Cortex

- [ ] **36. Snowflake Cortex: Fine-tuning, Search & Analyst [COF-C03]**
- ⭐⭐ [Cortex Fine-tuning](https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-finetuning) — 用自有数据微调 LLM
- ⭐⭐⭐ [Cortex Search](https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-search/cortex-search-overview) — 非结构化文本的混合（向量 + 关键词）搜索
- ⭐⭐⭐ [Cortex Analyst](https://docs.snowflake.com/en/user-guide/snowflake-cortex/cortex-analyst) — 业务人员用自然语言问结构化数据，自动生成 SQL
- 📝 [Q101](题库.md#q101) Cortex Analyst

### Streamlit（37 讲）
- [ ] **37. Streamlit**
- ⭐⭐⭐ [Streamlit in Snowflake](https://docs.snowflake.com/en/developer-guide/streamlit/about-streamlit) — 在 Snowflake 内创建和分享数据 / ML 的 Web 应用
- 📝 [Q11](题库.md#q11) Streamlit in Snowflake（S2-29，学完本讲更稳）

## セクション4: Account Access & Security

- ⭐⭐⭐ [Access Control](https://docs.snowflake.com/en/user-guide/security-access-control-overview) — RBAC / DAC、系统角色、角色继承
- ⭐⭐⭐ [Authentication](https://docs.snowflake.com/en/user-guide/admin-security-fed-auth-overview) — MFA、SSO、Key-pair、OAuth
- ⭐⭐ [Network Policies](https://docs.snowflake.com/en/user-guide/network-policies)
- ⭐⭐ [End-to-end Encryption](https://docs.snowflake.com/en/user-guide/security-encryption-end-to-end)
- ⭐⭐ [Encryption Key Management](https://docs.snowflake.com/en/user-guide/security-encryption-manage) — Tri-Secret Secure、密钥轮换
- ⭐⭐ [Column-level Security](https://docs.snowflake.com/en/user-guide/security-column-intro) — Masking Policy
- ⭐⭐ [Row Access Policies](https://docs.snowflake.com/en/user-guide/security-row-intro)
- ⭐⭐ [Object Tagging](https://docs.snowflake.com/en/user-guide/object-tagging)
- ⭐⭐ [Classification](https://docs.snowflake.com/en/user-guide/governance-classify-concepts)
- ⭐ [Access History](https://docs.snowflake.com/en/user-guide/access-history)
- ⭐ [Aggregation Policies](https://docs.snowflake.com/en/user-guide/aggregation-policies)
- ⭐ [Projection Policies](https://docs.snowflake.com/en/user-guide/projection-policies)

### 📝 本章题目（题库）

| 讲次 | 题号 | 知识点 | 还需 |
|---|---|---|---|
| S4 | [Q22](题库.md#q22) | ACCESS_HISTORY 影响分析 |  |
| S4 | [Q23](题库.md#q23) | Object Tagging 识别敏感数据 |  |
| S4 | [Q24](题库.md#q24) | Tri-Secret Secure | S2-11 |
| S4 | [Q25](题库.md#q25) | 列级血缘 ACCESS_HISTORY |  |
| S4 | [Q27](题库.md#q27) | Row Access Policy |  |
| S4 | [Q28](题库.md#q28) | Secondary Roles |  |
| S4 | [Q29](题库.md#q29) | 审计敏感数据访问 ⚠️ |  |
| S4 | [Q30](题库.md#q30) | Managed Access Schema |  |
| S4 | [Q31](题库.md#q31) | Masking Policy |  |
| S4 | [Q32](题库.md#q32) | MFA 第二因子方式 |  |
| S4 | [Q33](题库.md#q33) | SYSADMIN 权限范围 | S5 |
| S4 | [Q34](题库.md#q34) | Network Rules / Network Policies |  |
| S4 | [Q35](题库.md#q35) | IS_ROLE_IN_SESSION |  |
| S4 | [Q39](题库.md#q39) | Future Grants |  |
| S4 | [Q40](题库.md#q40) | Key-pair 密钥轮换 |  |
| S4 | [Q105](题库.md#q105) | Masking Policy + 角色继承 ⚠️ |  |
| S4 | [Q106](题库.md#q106) | 私钥加密 |  |
| S4 | [Q108](题库.md#q108) | 服务账号用 Key-pair |  |

## セクション5: Performance Concepts: Virtual Warehouses

- ⭐⭐⭐ [Overview of Warehouses](https://docs.snowflake.com/en/user-guide/warehouses-overview) — 尺寸、计费、auto-suspend / auto-resume
- ⭐⭐⭐ [Multi-cluster Warehouses](https://docs.snowflake.com/en/user-guide/warehouses-multicluster) — Maximized / Auto-scale、Standard / Economy
- ⭐⭐ [Warehouse Considerations](https://docs.snowflake.com/en/user-guide/warehouses-considerations) — Scale up vs scale out
- ⭐⭐ [Working with Warehouses](https://docs.snowflake.com/en/user-guide/warehouses-tasks)
- ⭐⭐ [Cost & Billing](https://docs.snowflake.com/en/guides-overview-cost) — Credits、Resource Monitors
- ⭐ [Query Acceleration Service](https://docs.snowflake.com/en/user-guide/query-acceleration-service)

### 📝 本章题目（题库）

| 讲次 | 题号 | 知识点 | 还需 |
|---|---|---|---|
| S5 | [Q63](题库.md#q63) | Query Acceleration Service 适用查询 |  |
| S5 | [Q68](题库.md#q68) | Economy 扩展策略 |  |
| S5 | [Q71](题库.md#q71) | 判断仓库过载（排队） |  |
| S5 | [Q79](题库.md#q79) | 多集群 Auto-scale + Standard |  |
| S5 | [Q81](题库.md#q81) | 按负载拆分仓库 |  |
| S5 | [Q82](题库.md#q82) | AUTO_SUSPEND 与仓库缓存 | S6 |
| S5 | [Q83](题库.md#q83) | MIN/MAX_CLUSTER_COUNT |  |
| S5 | [Q88](题库.md#q88) | Query Acceleration Service |  |
| S5 | [Q110](题库.md#q110) | QUEUED_OVERLOAD_TIME |  |

## セクション6: Performance Concepts: Query Optimisation

- ⭐⭐⭐ [Performance Optimization 总览](https://docs.snowflake.com/en/guides-overview-performance)
- ⭐⭐⭐ [Micro-partitions & Data Clustering](https://docs.snowflake.com/en/user-guide/tables-micro-partitions) — Pruning、clustering depth
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
