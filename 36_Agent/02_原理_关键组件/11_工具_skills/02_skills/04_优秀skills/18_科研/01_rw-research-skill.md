
https://github.com/rolandwonglonam/rw-research-skill

整个 18 个 Skill 不是一条固定流水线，而是一个按当前需求路由的图：

你的输入
    ↓
rw-research-router（判断当前卡点）
    ↓
选择一个主 Skill
    ↓
完成当前一步
    ↓
再次调用 rw-research-router
    ↓
选择下一个 Skill
比如：你有一组文献，不确定下一步做什么 → router 判断你需要先做证据图 → rw-evidence-map 处理证据关系 → 发现证据有冲突需要查创新点 → rw-research-novelty → 发现需要补充文献 → 回到 rw-literature-discovery。

每一步都是独立可运行的，不需要按固定顺序走完全程。

18 个 Skill，分阶段拆解
第一阶段：研究问题与文献
Skill	干什么
rw-research-router	判断现在卡在哪里，选择下一个合适的 Skill
rw-research-question	把模糊的研究兴趣，变成可检索、可证伪的研究问题
rw-literature-discovery	设计文献检索策略，核验来源是否可获取
rw-paper-extractor	从论文中提取研究设计、样本、测量、结果等结构化字段
rw-evidence-map	整理多篇文献的支持关系、冲突、偏倚和证据缺口
rw-research-novelty	从证据冲突中生成候选创新点，标出可行性和证伪条件
rw-review-methods	设计系统综述、范围综述和证据综合流程
第二阶段：研究设计与数据
Skill	干什么
rw-research-design	建立研究设计、样本、测量和分析计划
rw-research-data	检查数据、代码和补充材料的访问路径、版本和声明
rw-statistics-audit	核对分析单位、重复层级、统计方法和报告数字
rw-research-referee	在执行或投稿前，从审稿人视角攻击研究漏洞
第三阶段：写作、核验与投稿
Skill	干什么
rw-phd-write	根据证据结构组织论证，不补造来源
rw-phd-tone	提取并保留作者的学术语气，只改影响理解的部分
rw-claim-audit	逐条核验每个事实主张是否被原文支持（PASS/REVIEW/BLOCK）
rw-revision-patch	只修改批准的 Markdown 段落，其他内容保持不动
rw-journal-submission	核验目标期刊要求，准备投稿文件和审稿回复
rw-research-passport	保存跨对话、跨阶段的研究项目状态档案
rw-research-lab-router	根据任务和环境选择科研工具