# COF-C03 (SnowPro Core) 官方文档学习清单

按 Udemy 课程章节划分。⭐⭐⭐ 必读 / ⭐⭐ 重点 / ⭐ 补充

> 注：部分链接（Row Access、Query Profile、Semi-structured、UDF 等）非 Guides 首页直链，如打不开请在 docs.snowflake.com 搜索标题。

---

## セクション2: Snowflake AI Data Cloud Features and Architecture

按课程每一讲对应官方文档。讲次后的 ✅ 表示视频已看完（截至 7/28）。

### 架构（6～10 讲）
**6. What is Snowflake? ✅ ／ 7. Multi-cluster Shared Data Architecture ✅ ／ 10. Services Layer Overview ✅**
- [ ] ⭐⭐⭐ [Key Concepts & Architecture](https://docs.snowflake.com/en/user-guide/intro-key-concepts) — 三层架构、存算分离、shared-disk + shared-nothing
- [ ] ⭐⭐ [Supported Cloud Platforms](https://docs.snowflake.com/en/user-guide/intro-cloud-platforms) — AWS / Azure / GCP
- [ ] ⭐ [Supported Regions](https://docs.snowflake.com/en/user-guide/intro-regions)

**8. Storage Layer Overview ✅**
- [ ] ⭐⭐⭐ [Micro-partitions & Data Clustering](https://docs.snowflake.com/en/user-guide/tables-micro-partitions) — 列式存储、微分区 50～500MB（未压缩）

**9. Query Processing Layer Overview ✅**
- [ ] ⭐⭐⭐ [Overview of Warehouses](https://docs.snowflake.com/en/user-guide/warehouses-overview) — 虚拟仓库就是计算层（セクション5 会细讲）

### 版本与对象模型（11～17 讲）
**11. Snowflake Editions**
- [ ] ⭐⭐⭐ [Snowflake Editions](https://docs.snowflake.com/en/user-guide/intro-editions) — 哪个功能从哪个版本开始有（如多集群仓库、90 天 Time Travel 需 Enterprise）

**12. Snowflake Object Model**
- [ ] ⭐⭐ [Databases, Tables & Views](https://docs.snowflake.com/en/guides-overview-db) — Organization → Account → Database → Schema → 对象
- [ ] ⭐ [Securable Objects](https://docs.snowflake.com/en/user-guide/security-access-control-overview#securable-objects) — 对象层级图

**13. Organizations, Accounts, Databases & Schemas**
- [ ] ⭐⭐ [Organizations](https://docs.snowflake.com/en/user-guide/organizations) — ORGADMIN
- [ ] ⭐⭐ [Account identifiers](https://docs.snowflake.com/en/user-guide/admin-account-identifier) — orgname-accountname / account locator

**14. Table Types**
- [ ] ⭐⭐⭐ [Temporary and Transient Tables](https://docs.snowflake.com/en/user-guide/tables-temp-transient) — Permanent / Transient / Temporary 对比
- [ ] ⭐⭐ [External Tables](https://docs.snowflake.com/en/user-guide/tables-external-intro)
- [ ] ⭐ [Hybrid Tables](https://docs.snowflake.com/en/user-guide/tables-hybrid)

**15. View Types**
- [ ] ⭐⭐⭐ [Overview of Views](https://docs.snowflake.com/en/user-guide/views-introduction) — 普通 / Secure / Materialized
- [ ] ⭐⭐ [Secure Views](https://docs.snowflake.com/en/user-guide/views-secure)
- [ ] ⭐⭐ [Materialized Views](https://docs.snowflake.com/en/user-guide/views-materialized) — 需 Enterprise、单表、后台维护有费用

**16. Update: Worksheets vs. Workspaces**
- [ ] ⭐⭐ [Snowsight](https://docs.snowflake.com/en/user-guide/ui-snowsight)
- [ ] ⭐ [Worksheets](https://docs.snowflake.com/en/user-guide/ui-snowsight-worksheets)
- [ ] ⭐ [Workspaces](https://docs.snowflake.com/en/user-guide/ui-snowsight/workspaces)

**17. Databases, Schemas, Tables & Views - Hands-on**
- [ ] ⭐ [CREATE TABLE](https://docs.snowflake.com/en/sql-reference/sql/create-table) / [CREATE VIEW](https://docs.snowflake.com/en/sql-reference/sql/create-view) — 动手时查语法用

### C03 新增（18～20 讲）
**18. Open Table Formats & Apache Iceberg Table Type**
- [ ] ⭐⭐⭐ [Apache Iceberg Tables](https://docs.snowflake.com/en/user-guide/tables-iceberg) — External Volume、Snowflake 管理 vs 外部 Catalog
- [ ] ⭐ [Snowflake Open Catalog](https://docs.snowflake.com/en/user-guide/opencatalog/overview)

**19. Parameters ／ 20. Parameters Hands-on**
- [ ] ⭐⭐⭐ [Parameters](https://docs.snowflake.com/en/sql-reference/parameters) — Account / Session / Object 三类，层级覆盖关系
- [ ] ⭐ [SHOW PARAMETERS](https://docs.snowflake.com/en/sql-reference/sql/show-parameters)

### 函数与过程（21～24 讲）
**21. User Defined Functions (UDFs)**
- [ ] ⭐⭐⭐ [UDF Overview](https://docs.snowflake.com/en/developer-guide/udf/udf-overview) — SQL / JavaScript / Python / Java / Scala，标量 vs 表函数（UDTF）

**22. Stored Procedures**
- [ ] ⭐⭐⭐ [Stored Procedures Overview](https://docs.snowflake.com/en/developer-guide/stored-procedure/stored-procedures-overview) — 与 UDF 的区别、owner's rights / caller's rights

**23. UDFs, External Functions & Stored Procedures - Hands-on**
- [ ] ⭐⭐ [External Functions](https://docs.snowflake.com/en/sql-reference/external-functions-introduction) — API Integration、调用外部 API

**24. Sequences**
- [ ] ⭐⭐ [Using Sequences](https://docs.snowflake.com/en/user-guide/querying-sequences) — 不保证无间隙

### 管道（25～26 讲）
**25. Tasks & Streams**
- [ ] ⭐⭐⭐ [Streams](https://docs.snowflake.com/en/user-guide/streams-intro) — Standard / Append-only / Insert-only
- [ ] ⭐⭐⭐ [Tasks](https://docs.snowflake.com/en/user-guide/tasks-intro) — 定时、DAG、serverless 任务

**26. Dynamic Tables - Hands-on**
- [ ] ⭐⭐⭐ [Dynamic Tables](https://docs.snowflake.com/en/user-guide/dynamic-tables/overview) — target lag、与 Streams+Tasks 对比

### 工具与连接（27～30 讲）
**27. SnowSQL - Hands-on**
- [ ] ⭐⭐ [SnowSQL](https://docs.snowflake.com/en/user-guide/snowsql)
- [ ] ⭐ [Snowflake CLI](https://docs.snowflake.com/en/developer-guide/snowflake-cli/index) — 新一代命令行工具

**28. IDE Integrations - Visual Studio Code**
- [ ] ⭐ [Snowflake Extension for VS Code](https://docs.snowflake.com/en/user-guide/vscode-ext)

**29. Connectors, Drivers & Partnered Tools**
- [ ] ⭐⭐⭐ [Applications & tools for connecting](https://docs.snowflake.com/en/guides-overview-connecting)
- [ ] ⭐⭐ [Drivers](https://docs.snowflake.com/en/developer-guide/drivers) — JDBC / ODBC / Python / Node.js / Go / .NET
- [ ] ⭐ [Snowflake Ecosystem（合作伙伴工具）](https://docs.snowflake.com/en/user-guide/ecosystem)
- [ ] ⭐ [Developer](https://docs.snowflake.com/en/developer) — Snowpark、Streamlit、Native Apps

**30. Snowflake Scripting**
- [ ] ⭐⭐ [Snowflake Scripting](https://docs.snowflake.com/en/developer-guide/snowflake-scripting/index) — BEGIN…END、变量、游标、异常处理

### 计费（31 讲）
**31. Billing Overview**
- [ ] ⭐⭐⭐ [Understanding Overall Cost](https://docs.snowflake.com/en/user-guide/cost-understanding-overall) — 计算 / 存储 / 数据传输；Cloud Services 超过当日计算 10% 才收费
- [ ] ⭐⭐ [Cost & Billing](https://docs.snowflake.com/en/guides-overview-cost)

## セクション3: Snowflake AI Data Cloud Features and Architecture (AI Features)

- [ ] ⭐⭐⭐ [Snowflake AI & ML](https://docs.snowflake.com/en/guides-overview-ai-features) — Cortex AI Functions（COMPLETE / SUMMARIZE / SENTIMENT / TRANSLATE 等）、Cortex Search、Cortex Analyst、Document AI、ML Functions
- [ ] ⭐ [Snowflake Horizon Catalog](https://docs.snowflake.com/en/user-guide/snowflake-horizon)

## セクション4: Account Access & Security

- [ ] ⭐⭐⭐ [Access Control](https://docs.snowflake.com/en/user-guide/security-access-control-overview) — RBAC / DAC、系统角色、角色继承
- [ ] ⭐⭐⭐ [Authentication](https://docs.snowflake.com/en/user-guide/admin-security-fed-auth-overview) — MFA、SSO、Key-pair、OAuth
- [ ] ⭐⭐ [Network Policies](https://docs.snowflake.com/en/user-guide/network-policies)
- [ ] ⭐⭐ [End-to-end Encryption](https://docs.snowflake.com/en/user-guide/security-encryption-end-to-end)
- [ ] ⭐⭐ [Encryption Key Management](https://docs.snowflake.com/en/user-guide/security-encryption-manage) — Tri-Secret Secure、密钥轮换
- [ ] ⭐⭐ [Column-level Security](https://docs.snowflake.com/en/user-guide/security-column-intro) — Masking Policy
- [ ] ⭐⭐ [Row Access Policies](https://docs.snowflake.com/en/user-guide/security-row-intro)
- [ ] ⭐⭐ [Object Tagging](https://docs.snowflake.com/en/user-guide/object-tagging)
- [ ] ⭐⭐ [Classification](https://docs.snowflake.com/en/user-guide/governance-classify-concepts)
- [ ] ⭐ [Access History](https://docs.snowflake.com/en/user-guide/access-history)
- [ ] ⭐ [Aggregation Policies](https://docs.snowflake.com/en/user-guide/aggregation-policies)
- [ ] ⭐ [Projection Policies](https://docs.snowflake.com/en/user-guide/projection-policies)

## セクション5: Performance Concepts: Virtual Warehouses

- [ ] ⭐⭐⭐ [Overview of Warehouses](https://docs.snowflake.com/en/user-guide/warehouses-overview) — 尺寸、计费、auto-suspend / auto-resume
- [ ] ⭐⭐⭐ [Multi-cluster Warehouses](https://docs.snowflake.com/en/user-guide/warehouses-multicluster) — Maximized / Auto-scale、Standard / Economy
- [ ] ⭐⭐ [Warehouse Considerations](https://docs.snowflake.com/en/user-guide/warehouses-considerations) — Scale up vs scale out
- [ ] ⭐⭐ [Working with Warehouses](https://docs.snowflake.com/en/user-guide/warehouses-tasks)
- [ ] ⭐⭐ [Cost & Billing](https://docs.snowflake.com/en/guides-overview-cost) — Credits、Resource Monitors
- [ ] ⭐ [Query Acceleration Service](https://docs.snowflake.com/en/user-guide/query-acceleration-service)

## セクション6: Performance Concepts: Query Optimisation

- [ ] ⭐⭐⭐ [Performance Optimization 总览](https://docs.snowflake.com/en/guides-overview-performance)
- [ ] ⭐⭐⭐ [Micro-partitions & Data Clustering](https://docs.snowflake.com/en/user-guide/tables-micro-partitions) — Pruning、clustering depth
- [ ] ⭐⭐⭐ [Query Profile](https://docs.snowflake.com/en/user-guide/ui-query-profile) — Spilling、Exploding joins
- [ ] ⭐⭐⭐ [Persisted Query Results](https://docs.snowflake.com/en/user-guide/querying-persisted-results) — 结果缓存 24 小时
- [ ] ⭐⭐ [Clustering Keys](https://docs.snowflake.com/en/user-guide/tables-clustering-keys)
- [ ] ⭐⭐ [Search Optimization Service](https://docs.snowflake.com/en/user-guide/search-optimization-service)
- [ ] ⭐⭐ [Materialized Views](https://docs.snowflake.com/en/user-guide/views-materialized)
- [ ] ⭐ [Queries 总览](https://docs.snowflake.com/en/guides-overview-queries)

## セクション7: Data Loading & Unloading

- [ ] ⭐⭐⭐ [Data Loading 总览](https://docs.snowflake.com/en/guides-overview-loading-data) — Stage 类型（User / Table / Named / External）
- [ ] ⭐⭐⭐ [Bulk Loading from Local File System](https://docs.snowflake.com/en/user-guide/data-load-local-file-system) — PUT、COPY INTO、文件格式
- [ ] ⭐⭐⭐ [Snowpipe](https://docs.snowflake.com/en/user-guide/data-load-snowpipe-intro) — 自动加载、serverless
- [ ] ⭐⭐ [Data Loading Considerations](https://docs.snowflake.com/en/user-guide/data-load-considerations) — 推荐文件大小 100～250MB（压缩后）
- [ ] ⭐⭐ [Data Unloading](https://docs.snowflake.com/en/guides-overview-unloading-data) — COPY INTO &lt;location&gt;、GET
- [ ] ⭐ [Directory Tables](https://docs.snowflake.com/en/user-guide/data-load-dirtables)
- [ ] ⭐ [Connectors（Kafka 等）](https://other-docs.snowflake.com/en/connectors)

## セクション8: Data Transformations

- [ ] ⭐⭐⭐ [Semi-structured Data](https://docs.snowflake.com/en/user-guide/semistructured-intro) — VARIANT、FLATTEN、`:` 路径写法
- [ ] ⭐⭐⭐ [Streams](https://docs.snowflake.com/en/user-guide/streams-intro) — CDC
- [ ] ⭐⭐⭐ [Tasks](https://docs.snowflake.com/en/user-guide/tasks-intro) — 任务 DAG
- [ ] ⭐⭐ [Dynamic Tables](https://docs.snowflake.com/en/user-guide/dynamic-tables/overview)
- [ ] ⭐⭐ [UDF](https://docs.snowflake.com/en/developer-guide/udf/udf-overview) — 支持的语言
- [ ] ⭐⭐ [Stored Procedures](https://docs.snowflake.com/en/developer-guide/stored-procedure/stored-procedures-overview) — 和 UDF 的区别
- [ ] ⭐ [Unstructured Data](https://docs.snowflake.com/en/user-guide/unstructured-intro)
- [ ] ⭐ [Data Types](https://docs.snowflake.com/en/sql-reference/intro-summary-data-types)

## セクション9: Storage, Data Protection & Data Sharing

- [ ] ⭐⭐⭐ [Time Travel](https://docs.snowflake.com/en/user-guide/data-time-travel) — 0～90 天、UNDROP、AT / BEFORE
- [ ] ⭐⭐⭐ [Fail-safe](https://docs.snowflake.com/en/user-guide/data-failsafe) — 7 天，仅 Snowflake 可恢复
- [ ] ⭐⭐⭐ [Table Storage Considerations](https://docs.snowflake.com/en/user-guide/tables-storage-considerations) — Permanent / Transient / Temporary
- [ ] ⭐⭐⭐ [Cloning Considerations](https://docs.snowflake.com/en/user-guide/object-clone) — Zero-copy clone
- [ ] ⭐⭐⭐ [Secure Data Sharing](https://docs.snowflake.com/en/user-guide/data-sharing-intro) — Share、Reader Account
- [ ] ⭐⭐ [Collaboration 总览](https://docs.snowflake.com/en/guides-overview-sharing)
- [ ] ⭐⭐ [What are listings?](https://other-docs.snowflake.com/en/collaboration/collaboration-listings-about) — Marketplace
- [ ] ⭐⭐ [Replication & Failover](https://docs.snowflake.com/en/user-guide/account-replication-intro)
- [ ] ⭐ [Client Redirect](https://docs.snowflake.com/en/user-guide/client-redirect)
