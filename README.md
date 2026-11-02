# 高考阅读 C/D 篇外刊选材 / Gaokao CD Article Finder

[![validate](https://github.com/gaofred/gaokao-cd-article-finder/actions/workflows/validate.yml/badge.svg)](https://github.com/gaofred/gaokao-cd-article-finder/actions/workflows/validate.yml)
[![license: CC BY-NC-SA 4.0](https://img.shields.io/badge/license-CC%20BY--NC--SA%204.0-lightgrey.svg)](LICENSE)
[![python](https://img.shields.io/badge/python-3.9%2B-blue.svg)](https://www.python.org/)

搜适合改编成高考阅读 **C 篇（学术说明文）/ D 篇（思辨议论文）** 的外刊原文——按 CD 篇的选材口径筛，不是按"文章写得好不好"筛。

## 和通用外刊搜索的区别

| | 通用版（A/B 篇向） | 本 skill（C/D 篇向） |
|---|---|---|
| 体裁 | 新闻特写、feature | 研究报道、分析评论 |
| 结构要求 | 叙事流畅 | **有论证骨架**：claim → evidence → concession → conclusion |
| 改编目标 | 300–800 词直接用 | 必须能压到 **320–350 词、4–5 段** |
| 思辨性 | 不要求 | **D 篇硬性要求**（先立后破／让步转折／观点张力） |

一条纯事实快讯写得再无懈可击，也不入选——它没有论证骨架，压不出 C/D 篇。

## 六维筛选

1. **体裁对位**：C 篇＝研究报道／学术说明文；D 篇＝观点议论文／书评／深度分析
2. **论证骨架**：有明确的主张—证据—让步—结论链条
3. **思辨性**（D 篇必查）：先立后破、让步转折、张力结构，三者有其一
4. **改编可行性**：原文 500–1200 词，能压到 320–350 词 4–5 段且不丢主线
5. **事实密度**：有具名研究、数据、机构人名，供细节题与推断题取材
6. **课标主题吻合**：人与自然／人与社会／人与自我语境下的学术向子话题

## 工作流

```text
阶段一  搜索 → 初筛 → 推荐文档
        C 篇线（new study / researchers found）与 D 篇线（analysis / the problem with）分路并行
        取 10 篇候选 → 抓段落骨架 → 六维打分 → 精选
        🛑 展示推荐文档后暂停，等用户确认再进阶段二

阶段二  抓全文 → 生成 Word
        首抓即缓存：首次成功立刻落盘 /tmp/cd_articles_cache_<日期>.json
        后续只读缓存，不重复抓（Nature 二次抓取会被登录墙拦）
        scripts/create_docx.py 批量出 Word
```

**首抓即缓存**这条是踩过坑的：Nature 这类站点首次能抓、第二次被登录墙挡住，不缓存就会在阶段二丢全文。

## 产出

1. **推荐文档** `{topic}CD篇外刊推荐_YYYYMMDD.md`——每篇含标题／来源／日期／原文字数／目标篇位／难度／话题标签／中文摘要／**论证骨架一句话**／改编对接（建议框架、保留要点、预估成品）／原文链接。
2. **Word 全文**（用户确认后）——`scripts/create_docx.py` 批量生成。

## 安装

```bash
git clone https://github.com/gaofred/gaokao-cd-article-finder.git \
  ~/.workbuddy/skills/gaokao-cd-article-finder
```

## 依赖

- Python 3.9+
- `python-docx`：`pip install python-docx`（仅生成 Word 需要）
- 联网搜索与网页抓取能力（由运行环境提供）

## 目录结构

```
SKILL.md                  主流程：六维筛选、两阶段、F1–F10 失败降级、反例黑名单
references/
  cd-screening.md         C/D 双线关键词模板、六维评分明细、来源分级、论证骨架识别
scripts/
  create_docx.py          JSON → Word（单篇或批量）
tests/
  test_smoke.py           引用完整性 + 渲染冒烟
```

## 红线

- 不凑数：候选不足按降级交付并注明缺口，不塞不合格文章
- 摘要不充当全文：没抓到全文不进阶段二，预估字数须标「未核验」
- 不拿中文编译稿冒充原文
- 不虚构链接；staging／镜像／amp 域名先规范化再入库
- 不绕过付费墙，付费墙文章只列元信息备选
- 不把「有趣」当「适配」：故事性强但无论证的叙事文是 A/B 篇素材

## 与下游衔接

选材（本 skill）→ 改编 `text-adaptation-cd` → 评析 `cd-passage-review`

## 作者

Fred 老师 · 公众号：佛瑞德英语教学

## License

[CC BY-NC-SA 4.0](LICENSE)，禁止商业使用。课堂、校内、教研、非营利分享均可，需署名。详见 [NOTICE](NOTICE)。
