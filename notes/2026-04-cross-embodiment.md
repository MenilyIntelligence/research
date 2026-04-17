# Cross-embodiment transfer in task-level demonstration data

*April 2026*

## The problem, in one sentence

A demonstration of "pour water from a cup into a kettle" collected on a bimanual humanoid with 14 arm DoF should be usable — at least partially — to train a policy for a different humanoid with 12 arm DoF, or a wheeled mobile manipulator with a single 7-DoF arm. Today, it almost never is.

## Why it fails today

Three independent failures, all recoverable, compound into the current state:

### 1. Action space is implicit

Most published datasets record actions in whatever native format the data was captured in: joint positions at the capture robot's exact DoF layout, or end-effector poses in the capture robot's base frame. When you try to load this into a different embodiment, you have to guess:

- Is index 0 the right shoulder or the left shoulder?
- Is the gripper dimension binary, continuous, or force-normalized?
- Is the base frame world-fixed, robot-fixed, or task-origin?

Guesses are usually wrong, and wrong guesses silently degrade training rather than failing loudly.

### 2. Morphology is undocumented

A seven-DoF arm from Franka and a seven-DoF arm from UR are both "7-DoF arms", but their kinematic trees differ in shoulder roll order, elbow offset, and wrist axis sequence. Retargeting between them is feasible but requires the kinematic tree to be *declared*. Most datasets don't declare it.

### 3. Task representation is entangled with the source body

"Reach forward 30 cm" only makes sense if you know how long the arm is. A demonstration from a 1.8 m humanoid reaching for a cup is not directly usable for a 1.2 m humanoid even if the semantic task is identical. The fix is to represent the action in a **task-relative frame** (relative to the object, relative to the goal pose) rather than a body-relative frame wherever the semantic act permits.

## What a transferable demonstration looks like

Four annotations, made explicit at collection time:

```
{
  "action_space": "ee_6dof" | "joint_Ndof" | "whole_body_Mdof",
  "morphology": {
    "kinematic_tree_id": "humanoid_bimanual_14dof_v1",
    "dof_map": { "right_arm": [0..6], "left_arm": [7..13] },
    "link_lengths": { ... }
  },
  "reference_frame": "object" | "task_origin" | "robot_base" | "world",
  "retarget_hints": {
    "invariant_landmarks": [ "grasp_pose", "pour_pose", "release_pose" ]
  }
}
```

The `retarget_hints.invariant_landmarks` field is the key one. It marks the task-relevant key frames that any target embodiment must hit, and lets the rest of the trajectory be re-planned by the downstream embodiment's controller. This turns a rigid per-robot trajectory into a **waypoint schema with continuous interpolation**, which is the shape cross-embodiment transfer actually needs.

## How the schema supports this

[`menily/schema`](https://github.com/MenilyIntelligence/schema) includes `body.morphology` and `body.dof_map` as required fields precisely because cross-embodiment transfer is unrecoverable without them. The `action.space` field is a controlled vocabulary rather than free text. These are unsexy annotation choices that determine whether a dataset is worth anything six months after collection.

## How the toolkit supports this

[`menily/toolkit`](https://github.com/MenilyIntelligence/toolkit) ingests heterogeneous sources (POV video, VR hand-tracking, MoCap) and normalizes all of them into the same action-space + morphology annotation. The adapter does the work of converting source-native coordinate conventions into the schema's normalized form, so that downstream users don't have to know whether the demonstration originally came from a Quest hand-tracker or an OptiTrack rig.

## Open question

The hardest unsolved piece is **whole-body** transfer — locomotion-coupled manipulation where base motion and arm motion are tightly coupled. For pure tabletop manipulation, task-relative frames plus invariant landmarks are enough. For whole-body, the body itself is part of the task, and we do not yet have a clean decomposition. Our current view: retain per-embodiment whole-body demonstrations as a separate data class, and do not pretend they transfer.

---

*中文摘要：跨具身迁移今天几乎都失败。原因有三：动作空间隐含、机体形态未声明、任务表示与源机体耦合。解法是在采集时就显式标注 action_space / morphology / reference_frame / 关键路标（invariant_landmarks）。任务级轨迹一旦转为"带连续插值的路标序列"，跨具身迁移才具备结构性可行性。纯桌面操作可行，全身操作仍为开放问题。*
