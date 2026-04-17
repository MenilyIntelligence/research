# The data gap in embodied AI, stated precisely

*April 2026*

## The TL;DR

The bottleneck for generalist embodied agents in 2026 is not model capacity.
It is the shape, resolution, and diversity of the demonstration data we train them on.

**Models are running. Data is still asleep.**

## Why the usual benchmarks mislead

The public conversation around embodied AI datasets often reduces to hour-counts:
"100,000 hours of video", "10 million episodes", "1 billion frames". These numbers sound impressive and are trivially false as a measure of training utility.

A VLA model learning to *pour water from a cup into a kettle* does not improve from another million hours of unrelated human activity. It improves from:

1. A **natural-language goal** that matches the form of the downstream deployment interface.
2. A **visual context** at the ego or near-ego viewpoint the robot will actually occupy.
3. An **action sequence** at a resolution the policy's action head can consume (typically 10–30 Hz end-effector or joint trajectories).
4. **Task-level boundaries** — one segment = one semantic act — so that attention can attend to *this task*, not a sliding window of an adjacent task.

Large, unstructured video datasets fail on (1), (2), (3), and (4) simultaneously.

## What "task-level" actually means

A task-level demonstration is a four-tuple:

```
(goal_text, visual_context, action_trajectory, body_morphology)
```

Where:

- `goal_text` is a natural-language instruction, preferably in the language the model will be prompted in, with optional paraphrases for robustness.
- `visual_context` is a sequence of frames at the viewpoint the action was executed from.
- `action_trajectory` is a time series in a well-defined action space (end-effector 6DoF, joint N-DoF, or whole-body).
- `body_morphology` specifies the degrees-of-freedom layout so that cross-embodiment transfer is even attemptable.

If any one of these is missing or malformed, the demonstration is worth very close to zero for VLA training. This is why hour-counts are nearly meaningless.

## Where production data shortfalls bite in 2026

We have surveyed (informally) six pre-commercial VLA labs in Q1 2026. The recurring pattern:

1. **Collection is geographically concentrated.** A small team in one physical lab. Five to fifteen embodiments. Two to four environment types (kitchen, desk, factory bench).
2. **Schemas are incompatible across labs.** Each lab invents its own action encoding, its own goal-text convention, its own frame rate. Pooling is impossible without expensive post-hoc conversion.
3. **Body morphology is underspecified.** Many datasets don't annotate the DoF map, meaning cross-embodiment use is a manual rewrite per target robot.
4. **Language is thin.** A single instruction per episode, no paraphrases, often only English.

The result: VLA models have narrow competence and brittle generalization. Not because the architecture is wrong. Because the data they train on is narrow.

## What closes the gap

1. **A shared schema.** Not an "industry standard" from above — a working standard that converges because people actually pool data into it. See [`menily/schema`](https://github.com/MenilyIntelligence/schema) for our attempt.
2. **A distributed collection network.** Not one lab. Not one region. Contributors across multiple environments, languages, and body types.
3. **Format-preserving adapters** for heterogeneous inputs — POV video, VR hand-tracking, MoCap, teleoperation — so that the cost of contributing is low. See [`menily/toolkit`](https://github.com/MenilyIntelligence/toolkit).
4. **Open distribution channels** for the resulting pool.

None of these are algorithmic problems. They are infrastructure problems.

## What we are doing about it

Menily Intelligence is building the embodiment adapter layer between task-level foundation models and whole-body humanoid policies. The layer consists of:

- A schema (open)
- A toolkit (open-sourcing in stages)
- A globally distributed data collection network (in operation)

We are pre-MVP. We are small. We are building in open.

---

**Contact:** <Masashi@Menily.AI> · **Website:** <https://www.menily.ai>

---

*中文摘要：2026 年具身 AI 的瓶颈不是模型大小，是数据。过去的"小时数"指标几乎没意义。真正有训练价值的是"任务级示教数据"：自然语言目标 + 视觉上下文 + 动作轨迹 + 机体形态，缺一不可。当前行业的三大痛点：地理集中、格式互斥、形态缺漏。朔月智能正在通过 schema、工具链、全球分布式采集网络来弥合这个数据鸿沟。*
