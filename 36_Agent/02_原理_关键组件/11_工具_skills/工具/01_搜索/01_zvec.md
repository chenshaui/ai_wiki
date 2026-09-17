阿里 Zvec 团队开源 zg (zvec-grep)本地优先的搜索工具，面向开发者和 AI Agent。把语义检索、BM25、混合检索和 ripgrep 装进同一个 CLI/MCP 入口，让 Agent 用更少的工具调用和 token 找到散落在本地文件里的信息。
https://github.com/zvec-ai/zvec-grep

问题
- rg 在目标明确时快且准，但 Agent 越来越多地用自然语言找东西："恢复主题偏好"对应的实现可能叫 hydratePreferences，词面不重叠。
- 关键词匹配要么漏，要么结果爆炸且无排序。Agent 只能反复猜词、多轮搜索、逐个读文件拼上下文。

做法
- 四种检索一体：语义 / BM25 / 混合（RRF 融合）/ rg，对应"开放探索 → 收敛 → 精确验证"，Agent 可按线索直接跳到任一阶段。
- 结构感知：按代码符号、文档章节切成可寻址单元，保留路径位置；默认返回紧凑结果加预览，按需加载全文。
- 全程本地：扫描、Embedding、索引、检索都在设备端。内置 11 个本地模型，默认 potion-code-16m-v2（16M 参数，约 32 MiB，无需 GPU），Django 3457 文件在 M4 Pro 上索引不到 30 秒。远程 Embedding 需显式授权。
- 零配置：zg install 自动发现 Codex、Claude Code、Cursor、OpenCode 并配置 MCP；CLI 与 MCP 共用一份索引。
- 跨 macOS / Linux / Windows，索引嵌入式存储，无独立数据库，支持增量更新。

官方评测
配对 A/B，基线为 Agent 原生工具，zg 组仅增加预建索引、MCP 工具和使用指引。
- SWE-QA-Bench（20 题）：工具调用减半，输入 token 减近半，Judge 得分 +1.50。
- BrowseComp-Plus（80 题）：准确率 98.67% → 99.00%，token −37.6%，工具调用 −43.5%，耗时 −38.6%。

路线图
- 图搜索、查询规划与重排
- PDF / Word / PPT 与图片 OCR
- 持续压缩上下文
- 优化本地模型资源占用，探索 iOS / Android