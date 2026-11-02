# CD篇外刊搜索策略与筛选细则

## CD篇选材的底层逻辑

高考阅读 C/D 篇的本质是**学术素养测试**：考学生读懂"有人提出了一个观点/发现，用证据支撑，面对质疑，得出结论"这类论证型文本。所以选材的核心不是"话题好"，而是**文章本身有论证骨架**。

## 一、C/D 双线来源清单

### C篇线（研究报道 / 学术说明文）

| 来源 | 特点 | 注意 |
|------|------|------|
| ScienceDaily | 大学/期刊研究通稿，结构标准（发现→方法→意义） | 首选，论证骨架最规整 |
| Nature (news & features) | 顶级期刊新闻版，研究深度足 | 部分文章偏长 |
| Scientific American | 科学家撰稿，兼顾学术与可读 | 难度略高，适合拔高 |
| BBC Future | 科学向深度报道 | 部分偏软，需查论证密度 |
| The Guardian (Science section) | 研究报道+专家引语 | 注意区分报道与评论 |
| New Scientist | 研究综述向 | 部分需订阅 |

### D篇线（思辨议论文 / 深度分析）

| 来源 | 特点 | 注意 |
|------|------|------|
| Aeon | 学术向长文，哲学/心理/社会议题 | 篇幅常超2000词，须节选 |
| The Atlantic | 深度评论，观点鲜明 | 思辨性强，首选 |
| The Guardian (Opinion/Long reads) | 观点+叙事混合 | 选论证密度高的 |
| Nautilus | 科学人文交叉 | 题材契合高考"人与自然+思辨" |
| Psyche (Aeon sister site) | 心理学向实用哲学 | 话题贴近"人与自我" |
| The Economist (Leaders/Briefing) | 分析严密，结构教科书级 | 部分需订阅 |

### 三线（备选）
- The New York Times (Science/Opinion)、Washington Post、The New Yorker（部分付费）
- 注意排除：纯新闻通讯（Reuters/AP 快讯）、生活资讯类（BuzzFeed 类）

## 二、C/D 双线关键词模板

### C篇线（研究报道）

```
基础模板：
- "new study" OR "researchers found" OR "research reveals" {topic} 2025 2026
- "scientists discover" OR "study shows" {topic} site:sciencedaily.com
- {topic} research findings site:nature.com OR site:scientificamerican.com

子话题替换示例（行为心理）：
- "new study" OR "researchers found" behavior psychology decision 2025 2026
- "research reveals" habit OR motivation OR attention 2025 2026
```

### D篇线（思辨评论）

```
基础模板：
- {topic} essay OR analysis OR "the problem with" 2025 2026
- {topic} site:theatlantic.com OR site:aeon.co OR site:nautil.us
- {topic} "why we" OR "what if" OR "the myth of" opinion 2025 2026

子话题替换示例（AI伦理）：
- artificial intelligence ethics analysis site:theguardian.com 2025 2026
- AI "the problem with" OR "why we should" essay 2025 2026
```

### 轮次安排

1. **第1轮**：C篇线基础模板（广撒网）
2. **第2轮**：D篇线基础模板（广撒网）
3. **第3轮**：site: 限定权威来源（双线各一）
4. **第4轮**：针对前3轮有潜力结果的同话题替代文章

passage_slot=C 时 4 轮全走 C 篇线；slot=D 全走 D 篇线；slot=both 各 2 轮。

## 三、六维评分明细

每维 1-5 分，加权平均 ≥ 3.5 入选：

| 维度 | 权重 | 5分 | 3分 | 1分 |
|------|------|-----|-----|-----|
| 体裁对位 | 25% | 标准研究报道/议论文 | 资讯+观点混合 | 纯新闻/叙事 |
| 论证骨架 | 25% | claim-evidence-concession-conclusion 完整 | 有观点有证据但缺让步/反驳 | 无论证，纯陈述 |
| 思辨性 | 15% | 有先立后破或明显张力 | 有让步但浅 | 直线叙述 |
| 改编可行性 | 15% | 500-1200词，骨架清晰易压缩 | 1200-1500词或骨架需重组 | >1500词或结构松散 |
| 事实密度 | 10% | ≥3个具体研究/数据/人名 | 1-2个 | 无具体事实 |
| 课标主题 | 10% | 三大语境核心子话题 | 边缘子话题 | 超纲话题 |

D篇篇位要求：思辨性维度 ≥ 4 分，否则即使总分够也只可作 C 篇候选。

## 四、论证骨架识别要点

抓取全文骨架后，逐段首句扫一遍，找这些标记：

**C篇研究报道的标准信号**：
- "Researchers at [机构] found that..."
- "The study, published in [期刊], ..."
- "To test this, the team..."
- "The findings suggest..."

**D篇议论文的标准信号**：
- 开头：现象/争议引入（"For years, we assumed..."）
- 中段：让步（"Admittedly..." / "It is true that..."）→ 反驳（"But this overlooks..."）
- 结尾：升华但不喊口号（避免 "In conclusion, we should..." 的八股）

**淘汰信号**（命中即不入选）：
- 通篇是事件时间线（纯新闻）
- 无具名研究者/机构/期刊（不可考）
- 列表体（"5 ways to..."）
- 第一人称个人叙事（A篇体裁）

## 五、改编对接说明（对接 text-adaptation-cd）

推荐文档中"建议框架"字段按下表映射：

| 原文逻辑链 | 映射框架 |
|-----------|---------|
| 现象→研究→发现→应用 | Type A（自然理工·问题引入） |
| 发现→过程→阐释→前景 | Type B（自然理工·开门见山） |
| 理论→表现→影响→批判 | Type C（人文社科·理论） |
| 现象→观点→案例→反思 | Type D（人文社科·非理论） |

**压缩比经验值**：原文 800 词 → 成品 320-350 词最顺；原文 1200 词以上压到 350 会丢论证支线，需在"改编对接"里预警。

**亮点保留原则**（对接 text-adaptation-cd 的 constraints）：每篇推荐时标出 1-2 个"亮点表达"（直接引语、形象比喻、地道搭配），改编时必须保留。

## 六、与兄弟 skill 的协作链路

```
gaokao-cd-article-finder（本skill：选材）
    ↓ 推荐文档 + 全文 Word
text-adaptation-cd（改编：压到320-350词/4-5段/B2）
    ↓ 改编文本
cd-passage-review（评析：六维打分+红线自查）
    ↓ 评析报告
（可选）命题环节 → 出四道题
```

本 skill 的推荐文档格式已预留"建议框架 / 保留要点 / 预估成品"三个字段，交给 text-adaptation-cd 时无需二次分析。
