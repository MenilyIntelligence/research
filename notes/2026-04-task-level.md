# Task-level abstraction: why frame-level annotation breaks VLA

*April 2026*

## The claim

For vision-language-action (VLA) training, **frame-level annotation is the wrong unit of work.** Annotating individual video frames — with bounding boxes, object masks, verbs, or short captions — produces data that is cheap to collect but structurally unsuitable for policy learning. The right unit is the **task**: a semantically closed segment of behavior with a single language goal and a single action trajectory.

This note sketches why.

## What frame-level gets you

Frame-level pipelines borrow their shape from computer-vision supervised learning:

- One frame, one label (caption, action verb, mask)
- Temporal context implicit — "stack N frames, predict label for frame N+1"
- Boundaries decided post-hoc by a sliding window

For classification and detection, this is fine. For VLA, three things go wrong.

### 1. The action head gets the wrong target

A VLA action head produces a trajectory — a time series of joint or end-effector commands, typically at 10–30 Hz. Frame-level captions give it *verbs at timesteps*, not *trajectories with a goal*. The policy ends up treating language as a per-frame classifier rather than a long-horizon conditioning signal, and its generalization profile degrades accordingly: it recognizes "pouring" in a still image but cannot plan the eight-second trajectory that actually constitutes pouring.

### 2. Task boundaries are non-trivial and not recoverable by heuristics

"Where does the pour end and the set-down begin?" is a judgment call. Frame-level pipelines push it onto a post-hoc segmentation model, which is lossy, and the loss is systematically correlated with task difficulty (harder tasks have blurrier boundaries). Annotating at task-level from the start forces the annotator to make the call once, explicitly, and at the moment of maximum context.

### 3. Language at frame level is linguistically impoverished

Natural language describes **acts**, not frames. "Pour water from the blue cup into the kettle" is a six-word sentence that covers several hundred frames. A frame-level caption scheme either (a) repeats the same sentence per frame (wasteful, useless for training) or (b) degrades to per-frame verbs ("hold", "tilt", "pour", "retract") that don't match the language distribution a deployed VLA will receive from a human user.

## Cost argument (since it always comes up)

A common objection: frame-level labelling is cheaper per unit, so you get more volume. In our experience this is true in a misleading way.

- Frame-level: ~$0.02–0.05 per frame → for a 10-second 30 Hz clip, $6–$15 of labelling for a single demonstration
- Task-level: ~$1.50–$4.00 per task → same 10-second clip, one annotator pass, ~$2

Task-level labelling can be cheaper *absolutely*, and it produces data the VLA can actually consume. The "frame-level is cheaper" intuition comes from image-recognition economics that don't transfer.

## Implication for toolkit design

The implication for a preprocessing toolkit is concrete:

1. Every adapter emits **task segments**, not frame sequences with side annotations.
2. Segmentation is part of ingestion, not a downstream step.
3. Language is attached at the task level, with optional per-task paraphrases.
4. Frame-rate is chosen to match the **action space**, not the raw video's capture rate.

These are the constraints that drove the design of [`menily/schema`](https://github.com/MenilyIntelligence/schema) and [`menily/toolkit`](https://github.com/MenilyIntelligence/toolkit).

---

*中文摘要：帧级标注对 VLA 模型训练结构上不合适。问题有三：动作头拿到的是"时刻动词"而不是"带目标的轨迹"；任务边界由后置滑窗模型恢复必然损失，且损失与任务难度相关；语言在帧级别上被迫退化为单词级，脱离了部署时真正的人类语言分布。任务级标注的绝对成本反而更低，因为它一次到位。*
