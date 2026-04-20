# menily/research

[![License: CC BY 4.0](https://img.shields.io/badge/License-CC%20BY%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by/4.0/)
[![Updated: April 2026](https://img.shields.io/badge/updated-April%202026-blue.svg)](#notes)
[![Menily Intelligence](https://img.shields.io/badge/by-Menily%20Intelligence-black.svg)](https://www.menily.ai)

> Open research notes and draft papers on data infrastructure for embodied AI.
> Working documents, not polished publications.

## Table of contents

- [What this is](#what-this-is)
- [Draft papers](#draft-papers)
- [Notes](#notes)
- [Reading order](#reading-order)
- [Related projects](#related-projects)
- [Citation](#citation)
- [License](#license)
- [Contributing](#contributing)
- [中文简介](#中文简介)

---

## What this is

This repository holds the public research output of **Menily Intelligence (朔月智能)** — a data infrastructure company for vision–language–action (VLA) models and humanoid robotics.

Two kinds of material live here:

- **Draft papers** — arXiv-style preprints in PDF. Self-hosted, not yet submitted to arXiv.
- **Working notes** — short, dated, pre-arXiv scratch. What we are learning, stated explicitly.

This is not a paper archive and not a product-marketing blog. It is a **working research log**, published so that the design decisions behind `menily/schema` and `menily/toolkit` are legible and contestable.

## Draft papers

### Task-Level Demonstration Data for Vision-Language-Action Models: A Survey of Schemas, Adapters, and Cross-Embodiment Transfer

*Masashi. Menily Intelligence. April 2026. Draft v0.1. 12 pages.*

A survey of twelve task-level demonstration data systems (2023–2026), spanning trajectory-level datasets (Open X-Embodiment, DROID, BridgeData V2, OXE-AugE), motion-level datasets (BONES-SEED, AMASS, LAFAN1), and end-to-end VLA pipelines (π0, OpenVLA, GR00T N1, Gemini Robotics, Ψ₀).

The paper identifies the structural gap in the task-level semantic layer — no de-facto standard exists at this layer despite standardization at the layers above and below — and proposes **menily/schema v1** as a candidate specification with controlled vocabularies for action space, viewpoint, morphology, and data source.

📄 **[Download PDF (258 KB, 12 pages)](https://www.menily.ai/research/01-task-level-vla-data-survey.pdf)** ([HTML landing page](https://www.menily.ai/research/))

**40 references.** Covers all major systems 2023–2026. Self-hosted preprint (not on arXiv).

## Notes

Pre-arXiv scratch. One topic per note. Dated.

| Date | Title | File |
|---|---|---|
| 2026-04 | The data gap in embodied AI, stated precisely | [`notes/2026-04-data-gap.md`](notes/2026-04-data-gap.md) |
| 2026-04 | Task-level abstraction: why frame-level annotation breaks VLA | [`notes/2026-04-task-level.md`](notes/2026-04-task-level.md) |
| 2026-04 | Cross-embodiment transfer in task-level demonstration data | [`notes/2026-04-cross-embodiment.md`](notes/2026-04-cross-embodiment.md) |

## Reading order

If you are new to this work, we suggest this order:

1. **Notes first**: [Data gap](notes/2026-04-data-gap.md) → [Task-level abstraction](notes/2026-04-task-level.md) → [Cross-embodiment](notes/2026-04-cross-embodiment.md) — ~20 minutes total, sets the problem context.
2. **Then the survey**: [Task-Level VLA Data Survey (PDF)](https://www.menily.ai/research/01-task-level-vla-data-survey.pdf) — ~45 minutes, the full state of the field and our proposed schema.
3. **Finally the specs**: [`menily/schema`](https://github.com/MenilyIntelligence/schema) + [`menily/toolkit`](https://github.com/MenilyIntelligence/toolkit) — where our proposal gets implemented.

## Related projects

| Repo | Description |
|---|---|
| [menily/schema](https://github.com/MenilyIntelligence/schema) | Task-level VLA demonstration data specification |
| [menily/toolkit](https://github.com/MenilyIntelligence/toolkit) | Python reference implementation of the schema |
| [menily.ai](https://www.menily.ai) | Organization site — team, publications, contact |

### Additional public writings

Some of the more developed notes have been expanded into external technical articles:

- 📝 [VLA 任务级示教数据 schema 设计笔记：Menily/schema v1 规范与六字段解析](https://blog.csdn.net/2611_95864581/article/details/160290391) — CSDN, April 2026
- 📝 [异构机器人数据源统一接口的设计思路：menily/toolkit 的架构笔记](https://juejin.cn/) — 掘金, April 2026
- 📝 [给 VLA 训练数据设计一份 schema：六字段是怎么砍下来的](https://www.cnblogs.com/) — cnblogs, April 2026

## Citation

For the working notes collectively:

```bibtex
@misc{menily2026notes,
  author       = {{Menily Intelligence}},
  title        = {Research notes: data infrastructure for embodied AI},
  year         = {2026},
  url          = {https://github.com/MenilyIntelligence/research}
}
```

For the survey paper:

```bibtex
@misc{masashi2026tasklevel,
  author       = {Masashi},
  title        = {Task-Level Demonstration Data for Vision-Language-Action
                  Models: A Survey of Schemas, Adapters, and
                  Cross-Embodiment Transfer},
  year         = {2026},
  month        = {April},
  howpublished = {Menily Intelligence Research, self-hosted preprint},
  url          = {https://www.menily.ai/research/01-task-level-vla-data-survey.pdf},
  note         = {Draft v0.1}
}
```

## License

Creative Commons Attribution 4.0 International (CC BY 4.0) — see [LICENSE](./LICENSE) (added with first tagged release).

You are free to share and adapt the material for any purpose, including commercial, provided you give appropriate credit.

## Contributing

- 📧 **Factual errors or topic requests** → <Masashi@Menily.AI>
- 🐛 **Issues** → [open an Issue](https://github.com/MenilyIntelligence/research/issues/new) — especially welcome if you spot a missed reference or a judgment you disagree with
- 🔀 **Pull requests on notes** → we don't accept PRs on the notes themselves (they are our own running thoughts), but PRs that add references, fix typos, or improve diagrams are welcome

---

## 中文简介

`menily/research` 是**朔月智能的公开研究笔记仓库**。不是论文、不是产品文档，是我们在构建具身 AI 数据反射层时正在学习的东西。两类内容：

- **草案论文**：arXiv 风格的 PDF 预印本，自托管，暂未投稿。
- **研究笔记**：短、带日期、pre-arXiv 阶段的思考。

当前已公开：

- 📄 **[任务级 VLA 示教数据综述 PDF（12 页）](https://www.menily.ai/research/01-task-level-vla-data-survey.pdf)** — 2026.04 草案 v0.1，40 篇引用
- 📝 三篇 2026 年 4 月的研究笔记（数据缺口 / 任务级抽象 / 跨具身迁移）

欢迎通过 [GitHub Issues](https://github.com/MenilyIntelligence/research/issues) 指正事实错误或建议选题，直接反馈：<Masashi@Menily.AI>。
