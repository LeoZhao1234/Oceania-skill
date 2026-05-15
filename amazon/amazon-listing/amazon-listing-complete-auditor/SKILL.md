---
name: amazon-listing-complete-auditor
description: Full-spectrum Amazon listing audit combining AI Shopping readiness and search intent path analysis. Use this skill whenever the user wants to audit, diagnose, improve, or rewrite an Amazon product listing — whether the focus is on Alexa for Shopping/AI discovery, search keyword coverage, conversion optimization, buyer intent paths, title rewrite, bullet improvement, A+ content, backend keywords, or any general "check my listing" request. Trigger on: Amazon listing review, ASIN audit, product page analysis, listing optimization, Alexa for Shopping readiness, AI shopping assistant, search intent paths, keyword structure, or any request to evaluate or improve an Amazon product detail page.
---

# Amazon Listing Complete Auditor

## Overview

Audit any Amazon listing through two complementary lenses in a single pass:

1. **Search intent path coverage** — Can the right shoppers find this product at every stage of their search journey, from broad exploration through purchase to post-purchase?
2. **AI Shopping readiness** — Can Alexa for Shopping or other AI shopping assistants accurately understand, summarize, compare, and recommend this product?

These two lenses answer the fundamental questions every listing must pass:
- Can shoppers find it? (intent coverage)
- Once found, will AI and human shoppers choose it? (AI readiness + conversion)

## Input workflow

1. If the user provides an Amazon URL or ASIN, first try to open the URL directly and extract: title, images, price, rating, review count, bullets, description, A+ content, product details, variations, Q&A, reviews, and competitor modules.
2. If the URL is blocked, returns an error, or yields incomplete data, **fall back to the sorftime MCP tools** — do not ask the user for screenshots or manual input:
   - Call `mcp__sorftime__product_detail` with the ASIN and `amzSite` (default `US`) to get title, price, rating, review count, bullets, product attributes, category, sales data, and A+ status.
   - Call `mcp__sorftime__product_traffic_terms` to get the keywords the product ranks for, their search volumes, and natural vs. ad positions.
   - Call `mcp__sorftime__product_reviews` (reviewType `Both`) to get up to 100 recent reviews for sentiment and recurring themes.
   - Run all three calls in parallel for efficiency.
3. If the user provides only an ASIN (no URL), go directly to step 2.
4. If only partial content is available after all attempts, audit what exists and mark missing sections as "未提供 / 无法判断".
5. Do not invent claims, certifications, dimensions, materials, test results, or competitor data.

## Audit framework

### Layer 1 — Search intent taxonomy

Map how shoppers move through Amazon search using six transition types:

1. **Specification**: shopper adds modifiers — use case, attribute, brand, size, material, compatibility, demographic, or price ceiling
2. **Generalization**: shopper broadens after poor results, high price, or uncertainty
3. **Equivalence**: shopper searches the exact brand, model, SKU, variant, or known item phrase
4. **Substitution**: shopper swaps brand, material, feature tier, or product type to compare alternatives
5. **Complement**: shopper searches accessories, bundles, refills, replacement parts, or post-purchase needs
6. **Irrelevant**: listing attracts mismatched traffic that bounces or pollutes recommendation context

### Layer 2 — AI Shopping checklist

Verify the listing can answer these questions for Alexa for Shopping:
- What is this product and who is it for?
- What problem does it solve?
- What are the key specs (size, material, capacity, compatibility, limitations)?
- What makes it different from alternatives?
- What scenarios is it best suited for?
- What do reviews and Q&A reveal about real-world experience?
- Can Alexa for Shopping recommend it confidently using only this listing?
- Would natural-language shoppers surface it without exact keyword matches?

## Audit workflow

### Step 1: Identify the product mission
Summarize:
- Core category and primary job-to-be-done
- Likely buyer segments and purchase scenarios
- Price/quality positioning
- Main purchase objections
- Primary competitors or substitute categories

### Step 2: Build a search path map
Create 2–4 likely shopper paths. Label every transition with its intent type. Use arrows.

```
broad category term
  → [specification] size + use-case modifier
  → [equivalence] brand + model
  → [complement] replacement accessory
```

Always include:
- Normal purchase path: broad → specification → known item
- Comparison path: specification → substitution → equivalence
- Post-purchase path: primary product → complement chain

### Step 3: Score each dimension (see scoring rubric below)

### Step 4: Flag high-value intent dynamics when relevant
- **Saturation cliff risk**: too many specification steps before conversion → add comparison content
- **Post-purchase inversion opportunity**: product has accessories/refills → build complement keyword chain
- **Session reset risk**: vague compatibility attracts wrong buyers → add exclusion language
- **Substitution window**: competitors dominate branded terms → add non-disparaging comparison framing

## Required output structure

Output in Chinese unless the user requests otherwise. Use this exact structure:

---

# Amazon Listing 全维度检查报告：[产品名或 ASIN]

## 1. 总体结论
3–5 sentences. Diagnose the listing across both dimensions: search intent coverage and AI Shopping readiness. Name the most critical gaps that limit discoverability or recommendation probability.

## 2. 搜索路径地图
Show 2–4 shopper paths using arrows and intent type labels. Make this visual and scannable.

## 3. 综合评分表

### 搜索意图覆盖（0–5 分）
| 维度 | 分数 | 核心问题 | 首要修复 |
|---|---:|---|---|
| 泛化词捕获 | | | |
| 规格深度 | | | |
| 等价词捕获 | | | |
| 替代词防御 | | | |
| 互补词捕获 | | | |
| 无关流量过滤 | | | |
| 转化摩擦 | | | |

### AI 购物友好度（0–10 分）
| 维度 | 分数 | 判断 |
|---|---:|---|
| 标题清晰度 | | |
| Bullet 回答问题能力 | | |
| AI 可理解度 | | |
| 场景表达 | | |
| 参数完整度 | | |
| 差异化卖点 | | |
| Review/Q&A 支撑度 | | |
| 转化说服力 | | |
| SEO 与自然语言平衡 | | |
| Alexa for Shopping 推荐友好度 | | |

**综合得分：[X / 100]**
计算方式：搜索意图各项均值 × 10（满分50）+ AI友好度各项均值 × 5（满分50）

## 4. 核心问题清单

Group issues by priority. For each issue include: 问题 → 为什么重要 → 怎么改

**高优先级** — 直接影响 AI 推荐、搜索可见性或转化率
**中优先级** — 影响信任、可读性或差异化
**低优先级** — 表达、格式、补充信息类

## 5. 页面元素检查

Evaluate each element against both intent path coverage and AI readability. Note what's working and what needs fixing — don't just list problems.

### 标题
### 主图与副图
### Bullet Points
### 描述 / A+ 内容
### 后台关键词
### Variations
### Reviews / Q&A
### 广告关键词结构

## 6. AI 购物视角解读
How Alexa for Shopping would currently interpret this product based on the listing alone.

- **AI 已能理解的信息**
- **AI 可能无法准确回答的问题**（列出具体问题示例）
- **AI 可能不会优先推荐的原因**

## 7. 关键词分层建议
Organized by intent type. Adjust allocation ratios based on category norms and actual Search Query Performance data.

| 意图类型 | 建议词 / 短语示例 | 建议配置位置 | 参考权重 |
|---|---|---|---|
| 泛化词 | | 标题、后台 | ~15% |
| 规格词 | | Bullet、后台、标题 | ~60% |
| 等价词 | | 标题、后台 | ~10% |
| 替代词 | | Bullet、A+、后台 | ~10% |
| 互补词 | | A+、后台、Q&A | ~5% |
| 无关过滤词 | | 标题第一屏、Bullet 1 | — |

## 8. 可直接替换的新版 Listing

### Optimized Title
Follow Amazon readability conventions. Lead with core category + primary differentiator + top specification. No keyword stuffing.

### Optimized Bullet Points
5 bullets. Each starts with a clear benefit label in caps. Each answers a real shopper question.
- Bullet 1: What it is, who it's for, and the primary reason to care (generalization anchor)
- Bullet 2: Specification proof — key attributes that close the specification search
- Bullet 3: Compatibility, use-case proof, or scenario-based selling
- Bullet 4: Differentiation from substitutes — value framing, non-disparaging
- Bullet 5: Complement, bundle, care, warranty, or risk reversal

### Short Product Description
Conversion-focused paragraph. Natural language, not a spec sheet.

### A+ Content Module Suggestions
3–5 modules. For each: headline, body copy idea, and which intent type or AI question it answers.

## 9. FAQ / Q&A 问答库

At least 10 FAQs. Cover: sizing, compatibility, materials, use cases, care, warranty, comparison, limitations, and purchase objections.

| 买家问题 | 建议回答 | 意图类型 | 对 Alexa for Shopping 的作用 |
|---|---|---|---|

## 10. 运营落地清单

- **立即修改**（今天可执行）
- **需要补充素材**（本周内）
- **需要验证的数据**（下次更新前确认）
- **广告活动结构建议**（按意图类型拆分活动）
- **后续 7 天观察指标**

---

## Scoring rubric

**Search intent coverage (0–5 per dimension):**
- 5: excellent, captures the intent comprehensively
- 4: strong, minor gaps
- 3: acceptable baseline
- 2: partial but unclear or inconsistent
- 1: very weak, mostly absent
- 0: absent or actively harmful (e.g., attracting wrong traffic)

**AI Shopping readiness (0–10 per dimension):**
- 9–10: clear, specific, natural, buyer-question oriented, easy for AI to summarize
- 7–8: mostly clear but missing evidence, use cases, or FAQ coverage
- 5–6: understandable but generic, keyword-heavy, or incomplete
- 3–4: confusing, vague, or missing key product attributes
- 0–2: unusable, misleading, or severely incomplete

**Overall score formula:**
- Search intent: average the 7 dimensions (max 5), multiply by 10 → score out of 50
- AI readiness: average the 10 dimensions (max 10), multiply by 5 → score out of 50
- Total: add both → score out of 100

## Writing standards

- Be specific to the actual product — no generic SEO advice.
- Distinguish observed facts from assumptions when full listing data is unavailable.
- Do not recommend trademark misuse, false claims, review manipulation, or deceptive competitor comparisons.
- Avoid unverified absolute claims ("best", "guaranteed", "100%", medical/safety claims).
- Preserve brand positioning and factual claims from the source listing.
- For competitor brand terms, recommend compliant research and non-disparaging positioning only.
- Treat the listing simultaneously as a customer-facing sales page and an AI-readable knowledge base.

## Saving the report

After generating the full report, always save it as a Markdown file. Never skip this step.

- Filename: `{ASIN}_listing_audit.md` (e.g. `B0197V3OD2_listing_audit.md`)
- Save location: the user's current working directory, or a path they specified
- Content: the complete report exactly as generated — all 10 sections, tables, and rewritten copy
- After saving, tell the user the full file path

If the ASIN is not known (user provided a URL without an ASIN), derive a short slug from the product name for the filename.

## Reference

For the fast element checklist and keyword allocation guidance, see `references/checklist.md`.
