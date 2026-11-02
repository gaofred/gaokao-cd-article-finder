---
name: gaokao-cd-article-finder
description: 搜索适合改编成高考英语阅读C篇/D篇的外刊原文（学术向、有论证结构的说明文与议论文），按CD篇选材口径筛选，输出带改编对接信息的推荐文档和全文Word。触发场景："找CD篇外刊"、"找C篇/D篇语篇素材"、"找适合出CD篇的外刊"、"学术类阅读选材"、"研究报道/观点议论文外刊"。区别于 gaokao-article-finder（通用题材，A/B篇向），本skill专做CD篇（学术说明文/思辨议论文）选材。作者署名：Fred老师（公众号：佛瑞德英语教学）。
---

# Gaokao CD Article Finder — CD篇外刊选材器

## Overview

专门搜索适合改编为高考阅读 **C篇（学术说明文）/ D篇（思辨议论文）** 的外刊原文。与通用版 gaokao-article-finder 的核心区别：

| | 通用版（A/B篇向） | 本skill（C/D篇向） |
|---|---|---|
| 体裁 | feature/report 新闻特写 | 研究报道（research report）/ 分析评论（analysis, essay） |
| 结构要求 | 叙事流畅 | **有论证骨架**：观点—证据—让步—结论 |
| 改编目标 | 300-800词直接用 | 必须能压到 **320-350词、4-5段**（对接 text-adaptation-cd） |
| 思辨性 | 不要求 | **D篇硬性要求**（先立后破/让步转折/观点张力） |

选材标准与改编框架对齐 `text-adaptation-cd`，评析口径对齐 `cd-passage-review`。

## 参数说明

| 参数 | 说明 | 默认值 |
|------|------|--------|
| topic | 搜索话题（如"行为心理学"、"海洋酸化"） | 必填 |
| passage_slot | 目标篇位：C / D / both | both |
| count | 文章数量 | 5 |
| exclude_topics | 排除的话题（之前搜过的） | 无 |
| output_dir | Word文档输出目录 | `~/Desktop/<topic>_CD篇外刊/` |

## 工作流

### 阶段一：搜索 → 筛选 → 推荐

**Step 1: 去重检查**

读取工作记忆（`~/.workbuddy/MEMORY.md` 或工作区 `.workbuddy/memory/MEMORY.md`，优先后者）中 `[CD篇外刊搜索记录]` 节，排除已推荐文章。记录节不存在时按 F7 处理。

**Step 2: 多轮并行搜索**

使用网页搜索工具（WorkBuddy 为 WebSearch，其他 runtime 用同等能力），执行至少4轮并行搜索。关键词模板按 C/D 两条线分别给（详见 `references/cd-screening.md`）：

```
C篇线（研究报道）：
- "new study" OR "researchers found" {topic} 2025 2026
- "research reveals" OR "scientists discover" {topic} site:sciencedaily.com OR site:nature.com

D篇线（思辨评论）：
- {topic} analysis OR "the problem with" OR "why we" essay 2025 2026
- site:theguardian.com OR site:theatlantic.com OR site:aeon.co {topic} opinion analysis
```

**Step 3: 初筛（10→count）**

从搜索结果取10篇候选，用网页抓取工具（WebFetch 或同等能力）获取全文骨架（标题、各段首句、结论段）。

筛选标准（六维，详见 references/cd-screening.md 的评分明细）：
1. **体裁对位**：C篇=研究报道/学术说明文；D篇=观点议论文/书评/深度分析
2. **论证骨架**：有明确的 claim → evidence → concession → conclusion 链条
3. **思辨性**（D篇必查）：有先立后破、让步转折、张力结构之一
4. **改编可行性**：原文 500-1200 词为宜，能压到 320-350 词 4-5 段且不丢论证主线
5. **事实密度**：有具体研究、数据、人名机构名（供细节题/推断题取材）
6. **课标主题吻合**：人与自然/人与社会/人与自我三大语境下的学术向子话题

**Step 4: 精选并生成推荐文档**

精选 `count` 篇，生成 Markdown 推荐文档，文件名 `{topic}CD篇外刊推荐_YYYYMMDD.md`，每篇格式：

```markdown
## 文章 N

**标题**：{English Title}
**来源**：{Source}
**日期**：{Date}
**原文字数**：约 {word_count} 词
**目标篇位**：C篇 / D篇
**难度**：B2+ / C1-
**话题标签**：{tag1}, {tag2}

**摘要**：{中文摘要，100-150字}

**论证骨架**：{一句话画出结构，如"现象→两项实验→反驳质疑→结论"}

**改编对接**：
- 建议框架：Type A/B/C/D（对应 text-adaptation-cd 四框架）
- 保留要点：{必须保留的1-2个亮点表达或关键数据}
- 预估成品：约 {est_words} 词 / {est_paras} 段

**原文链接**：{URL}

---
```

**🔴 CHECKPOINT · 🛑 STOP：展示推荐文档后暂停，等用户确认再进入阶段二。** "只要推荐/不用抓全文"→到此为止；"抓全文/生成Word"→继续；未明确→停在阶段一并询问。

### 阶段二：全文抓取 → Word文档

**Step 5: 逐篇抓取全文**

用网页抓取工具（WebFetch 或同等能力）抓取全文。抓取指令中明确要求：
- 返回完整英文原文
- 去除广告、导航等无关内容
- 保留段落结构和标题

**首抓即缓存**：每篇首次抓取成功后，立即把全文落盘到 `/tmp/cd_articles_cache_<YYYYMMDD>.json`（含 title/source/date/url/body），后续所有步骤只读缓存、不重复抓取。实测教训：Nature 首次可抓、二次抓取被登录墙拦截——不缓存就会丢全文。

**Step 6: 生成Word文档**

用 `scripts/create_docx.py` 批量生成：

```bash
python3 scripts/create_docx.py --input-json <articles_json_file> --output-dir <output_dir>
```

Python 依赖 `python-docx`（缺失按 F8 处理）。调用前用 `python3 -c "import docx"` 校验；未安装则 `pip install python-docx`，或用你本地已装该包的虚拟环境解释器跑脚本。

**Step 7: 交付**

- 推荐 Markdown 文档 + 所有 Word 文档一起交付
- 更新工作记忆（格式见"去重策略"）
- 主动提示：这批文章可交给 `text-adaptation-cd` 做改编，改完可用 `cd-passage-review` 评析

## 失败模式与降级策略

| # | 触发条件 | 一线修复 | 仍失败兜底 |
|---|---------|---------|-----------|
| F1 | 某轮搜索结果 < 5 条或不相关 | 换子话题关键词重搜一轮（参照 cd-screening.md 子话题表） | 放宽年份再搜；仍不足 → 按实际数量交付，注明"候选不足" |
| F2 | 初筛合格候选 < count | 从初筛池递补下一篇最高分候选 | 递补后仍不足 → 按实际数量交付并注明缺口 |
| F3 | 抓取遇付费墙 | 放弃该篇，按 F2 递补 | 话题稀缺可列"备选"，标注"付费墙，仅元信息" |
| F4 | 抓取正文不完整 | 重试一次 | 仍不完整 → 按 F2 递补 |
| F4b | 抓取 URL 是 staging/镜像/跳转域名（如 www-stage.、amp.、idp. 重定向） | 先规范化为正式域名（去 stage 前缀、跟一次重定向）再抓 | 仍失败 → 按 F2 递补；禁止把 staging 域名写进推荐文档的链接字段 |
| F5 | 原文 > 1200 词 | 不入选，换下一篇 | 候选全部超长 → 至多保留 1 篇，在"改编对接"注明建议保留节；删减量 > 原文 70% 的须加 ⚠️ 压缩预警 |
| F6 | 字数/难度未经核验 | 该篇只列"备选"，不计入 count | 备选篇须标注"未核验" |
| F7 | 工作记忆不存在或无记录节 | 视为首次搜索，直接执行 | 交付后按格式创建记录节 |
| F8 | create_docx.py 报错（缺 python-docx） | `pip install python-docx`；或用本地已装该包的虚拟环境解释器重跑 `<venv>/bin/python scripts/create_docx.py ...` | 仍失败 → 全文降级为 Markdown 交付，告知原因 |
| F9 | 搜到的都是新闻快讯、无论证骨架 | 换 D 篇线关键词（essay/analysis/opinion）重搜 | 仍无 → 告知用户该话题学术向外刊稀缺，问是否放宽到通用版口径 |
| F10 | 文章看似学术但无具体研究/数据 | 检查是否有具名研究机构+可定位实验；无 → 降档为"观点散文"，只可作D篇候选且标注"证据密度低" | 全部为散文 → 按 F9 处理 |

## 去重策略

记录格式（写入工作记忆 `[CD篇外刊搜索记录]` 节）：

```markdown
### YYYY-MM-DD: {topic}（目标：C/D/both）
- {标题} | {来源} | {URL}
```

用户未指定排除时，自动排除最近3次搜索中已推荐的文章。**与通用版 gaokao-article-finder 的 `[外刊文章搜索记录]` 分开记**，互不干扰。

## 🔴 反例黑名单（禁止事项）

1. **禁止凑数**：候选不足按 F1/F2 降级交付，不得塞不合格文章
2. **禁止摘要充当全文**：未抓到全文不得进阶段二；预估字数须标"未核验"（F4/F6）
3. **禁止中文/编译文冒充原文**：发现聚合稿、翻译稿立即弃用
4. **禁止虚构链接**：URL 必须来自真实搜索/抓取结果
5. **禁止跳过去重检查**：Step 1 必读工作记忆（F7 兜底）
6. **禁止绕过付费墙**：付费墙文章只列元信息备选（F3）
7. **禁止未声明改参数**：确需缩减轮次/数量须在交付时声明
8. **禁止新闻快讯充数**：纯事实报道（无论证、无研究过程）不符合 CD 篇选材口径，哪怕写得再好也不入选
9. **禁止把"有趣"当"适配"**：故事性强但无论证的叙事文是 A/B 篇素材，转给通用版
10. **禁止 staging/镜像域名入交付物**：推荐文档和 Word 里的链接字段只写正式域名（www-stage.、amp.、idp. 跳转 URL 一律先规范化，见 F4b）

## Resources

### references/
- `cd-screening.md` — CD篇专属搜索策略：C/D 双线关键词模板、六维评分明细、来源分级表、论证骨架识别要点

### scripts/
- `create_docx.py` — Word 生成脚本（与通用版同款）
