# Sierra : Multi\-agent 是一个陷阱

![Image](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/authcode/?code=YzZhMzQxZGFmZWY5NThiNmIzZmZjMjgyNDJmY2ZjNjdfMDY2MzA5M2YwN2FkMDUzMDlmYTU0YTk0ZWEzYmJiNTRfSUQ6NzY1Nzg0NTU2MjYxMjA5MTg2NV8xNzgyOTgxMTM5OjE3ODMwNjc1MzlfVjM)

# “

企业级 Agent 的真实形态，可能和多数 demo 正好相反。

一个 demo 里，Agent 最吸引人的部分通常是「它能自己做多少事」。但在 Sierra 的生产系统里，最值钱的地方是另一套东西：低延迟语音、多模型并行、支付隔离、企业变更审核、持续监控，以及一套能让业务人员直接改 Agent 行为的 no\-code 层。

Sierra 是 Bret Taylor 和 Clay Bavor 创立的企业 AI Agent 公司，方向是帮大公司搭建面向客户的 Agent。LangChain 的 Max Agency 访谈了 Sierra 产品负责人 Zack Reno Wedeen。公开报道里，Sierra 已经是估值约 100 亿美元、ARR 过亿美元级别的公司；访谈里 Zack 提到，Sierra 服务着 most of the Fortune 20，以及 40% 到 50% 的 Fortune 50 或 Fortune 100。

这个量级决定了它不会按 demo 的逻辑做 Agent。问题变成了另一种形态：客户在航班改签、会员续费、商品推荐、支付、退款、取消订阅时，Agent 能不能在 1 到 2 秒内给出可靠动作，还能被企业审计、回滚和持续优化。

---

### 客服只是第一个入口

Sierra 常从 customer service 开始，是因为大企业采购时会带着很具体的 RFP：我们要解决客服问题。

但 Zack 的定义涵盖更多。他说 Sierra 是覆盖客户关键时刻的完整互动平台。以航空公司为例，一个客户可能先浏览航班，再订票、选座、加宠物进客舱，之后遇到延误、取消、改签、行李问题。这些动作有的像销售，有的像服务，有的像会员运营，但它们都属于公司和客户的关系。

Agentic Commerce 就是 Sierra 的下一个赌注。Zack 的判断是：它会比 e\-commerce 更大。

这里的 Agentic Commerce 不是在品牌官网右下角多放一个聊天框。更可能出现的形态是，你的个人 Agent 去找 Redfin 的 Agent、航空公司的 Agent、运营商的 Agent、银行的 Agent，帮你查房源、订服务、取消订阅、处理售后。用户不再逐个打开网站点击，品牌也不再只面向人的眼球设计页面。

这一层变化会让 Agent 从「客服成本优化工具」变成商业入口。已经有 Sierra Agent 因为完成销售而拿佣金。客服 Agent 一旦开始影响收入，企业对它的要求就不再停留在「回答得像人」，还要能达成业务结果。

### 支付能力决定 Agent 能走多远

![Image](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/authcode/?code=ZThmMzFhNDEzMDkxMzA0MmQ1Y2MwMmE1ODI2YmU0NjZfYzNiZDUyMmU5NDM0MGFkMTQxOGNjMjFkMGEwOWQxZWRfSUQ6NzY1Nzg0NTU1NTcyOTY5ODAzOV8xNzgyOTgxMTM5OjE3ODMwNjc1MzlfVjM)

如果 Agent 只回答问题，系统边界相对简单。一旦 Agent 要收钱，问题立刻不同了。

Sierra 提前投入了支付基础设施，并拿到 PCI DSS Level 1 认证。Zack 说，他们有隔离基础设施，支付信息不会进入外部大语言模型，因为 LLM provider 并没有以这种方式完成 PCI 认证。

这句话揭示了企业级 Agent 和普通 demo 的核心差距。demo 可以让模型「看起来像在结账」，生产系统必须回答更具体的问题：信用卡信息在哪里处理，哪些 token 进入模型，哪个 provider 能碰到什么数据，出了问题谁负责，企业安全团队怎么验收。

所以 Agentic Commerce 首先是一组后端基础设施问题。品牌要让 Agent 代表自己卖东西，就像过去要接入 Shopify、Stripe、推荐系统和风控系统一样，需要一套能表达商品、偏好、支付、合规和品牌策略的平台。

只要你的 Agent 进入商业闭环，瓶颈会从模型能力扩展到支付、权限、审计、数据隔离和责任边界。

### 语音让 Harness 变成实时系统

![Image](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/authcode/?code=N2JlMjY2MTFmYTY3NGViN2U5ZTY3ZTFhYmJlYjg2MGVfYWY0NzI5ODk4Nzg0NWVhZDIzN2QzMDI3YTIyNWZkM2ZfSUQ6NzY1Nzg0NTU1NDk5OTI2NjI1Ml8xNzgyOTgxMTM5OjE3ODMwNjc1MzlfVjM)

Sierra 的大多数对话是 voice。语音和聊天最大的差别是延迟。

用户在聊天窗口里等几秒，还能接受。电话里 1 到 2 秒没人回应，就会觉得对方消失了。于是 Sierra 的 Agent Harness 不能像编码 Agent 那样慢慢思考、慢慢查文件、慢慢跑工具。它必须在用户说话时就开始准备下一步。

延迟压力正是 Sierra 大量投入 parallelism 的原因。Sierra 会在还没完全确定问题是否需要知识答案之前，先把知识检索跑起来。系统可能一边判断问题类型，一边准备回复。一次 conversation turn 里，可能会调用 10 到 15 个模型：少数 frontier model 负责高价值推理，一批 classifier 负责分类，还有一批专门为语音场景做 speculative execution。

语音转写也不是选一个模型就结束。Zack 举过一个例子：北英格兰某些口音下，一个模型转写质量最高，但静音时容易幻觉；另一个模型判断静音更可靠。Sierra 会并行跑两个模型。如果静音模型判断是 silent，就信它；如果不是 silent，就信转写质量更好的模型。

这个例子不起眼，却能说明生产 Agent 的基本形态。系统要承认每个模型都有边界，再用并行、路由、ensemble 和 eval 把它们组合起来。

Voice\-to\-voice model 也是同理。Zack 认为未来多数语音 Agent 会用原生语音模型，但现在只适合流程相对简单、自然度比复杂工具调用更重要的场景。因为它们更贵，推理和工具调用还没那么可靠，也还覆盖不了全球企业需要的所有语言。

### 最好的上下文是刚刚够用

Zack 对 Context Engineering 的定义是：给 Agent 展示完成任务所需的一切，但不要更多。

早期的 Sierra Agent SDK 更像把上下文一口一口喂给模型。模型变强以后，可以把「该给什么」和「不该给什么」的边界放宽一点，但原则没有变。上下文要相关、及时、一致。

Zack 特别提到 progressive disclosure。不要在上下文还不相关时提前塞进去，也不要在上下文仍然相关时粗暴拿走。做 prompt compaction 时，如果把关键信息压丢，或者保留了一段和当前 system prompt 冲突的历史，模型就会出错。

**💡**很多团队会把这种错误归因给模型不聪明。Zack 的判断是：每次你觉得模型很笨，很可能问题在你这边。

Sierra 对 prompt caching 的态度：缓存能省成本、提速度，但质量优先。Zack 说，如果一次客户对话可能卖出 100 美元产品，或者 1000 美元 lifetime value 的计划，团队就有空间把质量放在缓存命中率之前。

团队应该先估算一次成功对话值多少钱，再决定要为速度、省 token、缓存稳定性付出多少工程复杂度。

### 多 Agent 不是默认答案

Zack 认为，很多团队太早开始用多 Agent。

如果拆多 Agent 的原因是「一个团队负责一个 Agent，另一个团队负责另一个 Agent」，那是在把组织架构发到生产环境里。如果拆分只是因为人类更习惯把问题分开，也未必是在优化结果。

一个 Agent 做 triage，另一个 Agent 执行任务，看起来职责清晰。但执行任务的 Agent 可能拿不到 triage 过程里的关键信息，triage Agent 又拿不到任务流程里的细节。最后两个 Agent 都少了一块上下文，质量反而下降。

适合拆多 Agent 的情况存在，比如两个任务的上下文可以彻底分离。但以 2026 年 5 月的状态，为了「提高质量」而拆多 Agent，很少能真的奏效。很多问题更适合用更好的 Context Engineering 解决。

这条判断反对的是把人类管理系统里的分工直接搬到模型系统里。Agent 不需要和公司组织图长得一样，它需要和任务的信息流长得一样。

### 持续学习也要有人按下发布键

![Image](https://internal-api-drive-stream.feishu.cn/space/api/box/stream/download/authcode/?code=YzQyNDExMGQwZDJhZDYxMDU1NmE1ZjQzZDU4OTc2M2VfYzUzODdmM2FmODU1YWFjNGYwYTQ1YWMxNjBjZTFkNGFfSUQ6NzY1Nzg0NTU1NTQ5NDE2MTYxMl8xNzgyOTgxMTM5OjE3ODMwNjc1MzlfVjM)

Sierra 平台里有三个关键动作：Analyze、Build、Release。

Analyze 里有 Explorer、reports 和 monitors。Explorer 像面向客户对话和业务数据的 Deep Research。运营人员可以问，为什么 resolution rate 下降，怎么提高销售转化，为什么试用用户没有转成付费。Monitors 则像一直跑在每段对话上的 evaluator，负责发现问题、标记 review 或生成 issue。

Build 里有 Ghostwriter 和 Journeys。Ghostwriter 类似 Codex 或 Claude Code，但它写的是 Sierra 的 no\-code Agent 规格。Journeys 更像自然语言 SOP 加声明式规则，可以确定性地编译到 Agent SDK code。业务人员能直接描述 Agent 应该怎么处理退货、改签、推荐和升级，工程团队也能通过 Git、CI/CD 和代码扩展复杂工具。

Release 是企业客户最在意的部分。大客户不会让 Agent 自己随便改生产行为。Sierra 支持协作、审核、模拟测试、变更管理和发布流程。Zack 说，现在的持续学习状态是：Monitor 可以自动发现 issue，Ghostwriter 可以自动建议 fix，人类 review 后再推到 Agent。

未来有些低风险改动可能只发 FYI。比如知识库里有明显矛盾，Agent 查官网确认了正确答案，就可以自动修。但 Sierra 没有急着推进到那一步。多数客户仍然希望 review 每一次进入 Agent 的变更。

企业级 Agent 的「自我改进」要把人类从读 10000 段对话，变成只读 5 段需要判断的对话。自动化的价值在于压缩审批范围，把人的判断留给最值得看的地方。

### 结果定价会重写产品优先级

Sierra 最有代表性的商业设计，是 outcome\-based pricing。

如果 Agent 只是做知识问答，它更像 commodity，按 usage 或 seat 收费更简单。但如果 Agent 能卖出会员、完成留存、处理复杂设备故障、促成购车线索，它创造的是业务结果。客户愿意为结果付费，Sierra 也因此和客户站到同一边。

这是 Sierra 认为自己成功的原因：outcome\-based pricing 减少了企业合作里最常见的优先级摩擦。双方不再围绕「用了多少 token、多少 seat、多少小时」争论，而是围绕结果优化。

按 token 收费的 Agent 产品，会天然关注调用量和成本。按 outcome 收费的 Agent 产品，会更关心业务闭环、转化率、可靠性、支付、监控和客户生命周期数据。

所以 Sierra 的 Agent Data Platform 也不只是「记忆」。它把客户数据平台、内部系统和 Sierra 对话上下文接起来，让 Agent 不只理解此刻用户说了什么，还知道应该给什么推荐、用什么优惠、如何权衡两个 offer，以及怎样让一次销售或留存动作更自然。

LLM 擅长当下的共情和表达，上一代推荐系统更懂结构化偏好。Sierra 想做的是把两者接起来。

### 产品判断变得更频繁了

访谈最后，Zack 谈到 Sierra 喜欢什么样的人。

编码 Agent 让写代码更快，review 也更快。但产品判断和客户直觉没有因此变少，反而更频繁地成为瓶颈。他用了一个赛车类比：车越快，进站换胎越频繁。AI 让团队跑得更快，于是判断方向、理解客户、协调沟通的动作也要更密。

Sierra 的 AI\-native interview 会让候选人在几小时内用 AI 构建一个端到端产品，再和团队 review。这个过程能看出一个人把什么视为自己的控制范围，能不能主动把「看似不归我管」的问题拿进来解决。

企业级 Agent 不缺能跑起来的 demo，缺的是把客户语境、业务结果、工程约束和安全边界同时放进系统的人。

最好的 AI Agent 看起来可能更简单。它不急着拆成一堆 Agent，不急着把所有东西塞进 prompt，不急着让模型自己改生产环境。它把上下文放准，把延迟压住，把支付隔开，把监控跑起来，把人放在该判断的位置。

# “

这不是更保守。Agent 要进入真实商业流程，系统必须先学会克制。

### 参考资料

\[1\]https://youtu\.be/uCKhOmth2ms

https://mp\.weixin\.qq\.com/s/Q\-OLpZfH5Ze\-S\_XqZ2gKMA

