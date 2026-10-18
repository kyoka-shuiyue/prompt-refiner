# 更新记录 / Changelog

## 1.3.0 — 2026-10-18

本次将最新本机技能同步到 GitHub；对比远端 `bb66e0d` 中的 1.2.0。

### 方向

重点转向理清已有思路：辨认意图、解决重大歧义和冲突、补齐必要目标，然后及时回到原任务。明确任务不再因长度或复杂度进入精炼流程。

### 删除与合并

- 移除技能自身的强制计划文件门槛，把计划、进度、实现和成品验证交回原任务工作流。
- 删除参考文件中重复的口令规则和执行计划章节，触发、路由、口令及交接规则统一在 `SKILL.md` 维护。
- 移除六种内部模式标签、每阶段重复漂移检查，以及 Inspect / Verify / Change 三层执行流程，保留必要权限边界。
- README 移除擅自补入功能、性能数字和实现路线的示例，以及与当前使用说明无直接关系的理论宣传段落。

### 保留与修正

- 保留 Prompt 创建、改写、点评、比较，以及目标、完成条件、重要边界、停止规则和重大未决选择的检查。
- 新增统一触发门槛：明确任务、清晰创作、仅缺输入、环境未知默认不触发；明显转写错误直接修正。
- 明确目标补全的边界：从既有目的推导必要质量条件，不能添加额外功能、模块、业务承诺、固定字数或创作方法。
- 重大歧义给出 2–3 个具体选项。集中一轮是默认方式，不是硬上限；实质冲突未解决时做最小追问。
- 区分“继续”与“不要问”，沿用已有授权，不借口令扩大外部操作权限。
- 增加退出与重入规则：意图与关键边界清楚后结束；普通阶段切换、缺文件或技术失败不重新访谈。
- 版本号改放在 frontmatter 的 `metadata.version`，同步界面元数据和中英文 README。

### 验证范围

发布检查覆盖本机三个技能文件与仓库副本的 SHA-256 一致性、技能结构、界面 YAML、相对链接与 Git 差异。历史评测资料保留并标明适用提交。本次未进行新版多轮行为对照测试，不将旧分数或预期提效作为新版实测结论。

### English

This release synchronizes the latest local skill with GitHub, compared with version 1.2.0 at `bb66e0d`.

- Focuses activation on unclear intent, material conflicts, unclear deliverables, and explicit prompt work. Clear tasks and creative briefs proceed directly.
- Removes the mandatory plan-file gate, duplicated reference rules, internal mode labels, recurring phase checks, and the three-level execution workflow. The underlying task owns planning, implementation, and validation.
- Retains prompt creation and editing, necessary goal completion, material questions, permission boundaries, and stop rules. Adds explicit refinement exit and re-entry criteria.
- Limits additions to what follows from the user's existing purpose. No invented features, business commitments, fixed metrics, or prescribed creative methods.
- Uses concrete options for material ambiguity. One compact question round is the default, with minimal follow-up when necessary. Distinguishes continuing an agreed task from delegating choices.
- Updates interface metadata and both READMEs; removes examples that invented requirements and unrelated promotional exposition. Stores the version under `metadata.version`.
- Checks synchronization, structure, YAML, relative links, and diffs. Historical evaluations remain historical; no new comparative behavioral scores or measured efficiency gains are claimed.
