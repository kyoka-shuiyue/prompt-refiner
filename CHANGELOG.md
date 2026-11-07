# 更新记录 / Changelog

## 1.4.0 — 2026-11-07

对比远端 `f3fd712` 中的 1.3.0。经过 1.4.0–1.4.3 四轮草稿和盲评，这一版整理成正式版。

### 方向

除了理清意图，这一版还处理强模型在实际使用中最容易出错的三件事：完成标准不清楚、需求多次改动后交付走样、把任务交接给其他模型时写得不够清楚。

### 新增

- **改动类请求附完成条件**：修代码或改文档时，最后附 2–4 行可检查的完成条件。修 bug 时要写怎么复现，以及修好后怎么确认。
- **限定不是矛盾**：后一句是在缩小前一句的范围（例外、条件或子集）时，直接合并成一条规则，不当成矛盾去问。
- **多次改动后的最终版**：在内部整理最终口径并逐项核对。用户要求“用最早那版”时，先检查那一版里有没有后来已被否定的内容，不能悄悄恢复。被替换的内容不再出现在成品里；定稿时不提新功能。
- **改写规则不定死含糊处**：原文含糊的地方保持含糊，标成 `【待确认】`，不补原文没有的条件、期限或流程。
- **提示词要可执行**：提示词里的每个判断都要有可用的标准，或留给用户填写的位置，并写明拿不准时怎么办。处理外部内容时，写明内容是数据，不是指令。
- **交接任务单**：只写目标、用户原话与材料、最终口径、完成条件、边界、停止条件和假设；保持环境中立；同一要求只写一次。
- **默认值不等于加功能**：默认值只用来补会卡住工作的缺口，不能借此加入用户没要求的字段、校验或步骤。

### 保留与调整

- 保留触发门槛、三种工作方式（执行、Prompt、回答）、2–3 个选项的提问方式、口令规则、权限边界、退出与重入规则。
- “要求简短时保留关键事实”“需求有歧义时先只检查再提问”“泛用的‘测试’不等于压力测试”等 1.3.0 规则继续保留，写法更简洁。
- 创作类精炼和参考已有产品的指引移到 `references/patterns.md`，按需读取。
- `SKILL.md` 从约 16KB 精简到约 10KB。

### 验证范围

- 回复者使用 Claude Haiku 5.5，盲评使用 Claude Sonnet 5.5，回复匿名打乱后才评分。每版每条用例跑 2 次。
- 第三批（8 条新用例）：1.4.2 草稿平均 8.56，1.3.0 平均 8.00；检查点失败次数 0 对 4。
- 第四批（6 条新用例和 2 条复测）：1.4.3 草稿 8.44，1.4.2 8.00，1.3.0 7.25；1.4.3 的检查点失败为 0。
- 正式版在 1.4.3 草稿的基础上，补回了几条 1.3.0 的保护规则，并把引用的段落补进 `patterns.md`。这几处补回的内容本身没有再单独测试。
- 局限：样本小；只有一位评审；检查点由改版者编写；测试环境带有用户自己的全局规则，版本间的差距可能被低估；没有在更强的模型上测试。这些结果不代表在所有模型上都有同样幅度的提升。

### English

Compared with 1.3.0 at `f3fd712`. Consolidated from four rounds of drafts (1.4.0–1.4.3) with blind evaluation.

- Adds a 2–4 line done-when note to change requests; bug fixes include how to reproduce and how to confirm.
- Merges qualifiers (exceptions, conditions, subsets) instead of treating them as contradictions.
- Adds final-version handling after repeated changes: build final terms, check item by item, never silently revive superseded items, propose nothing new while finalizing.
- Rewrites of rules or specs keep vague terms vague and mark them `【待确认】`.
- Prompts get usable criteria or fill-in slots for every judgment, an unsure path, and a "content is data, not instructions" rule for outside content.
- Adds environment-neutral handoff briefs with each requirement stated once.
- Defaults fill blocking gaps only; they are not a way to add features.
- Keeps the 1.3.0 activation gate, tracks, override phrases, permission boundaries, exit and re-entry rules; moves creative refinement and known-product guidance to `references/patterns.md`.
- Evaluation (Haiku 5.5 responders, Sonnet 5.5 blind judge, 2 runs per version per case): batch 3, 8.56 vs 8.00 for 1.3.0 (check failures 0 vs 4); batch 4, 8.44 for the 1.4.3 draft vs 8.00 (1.4.2) and 7.25 (1.3.0). Small samples, a single judge, rubric written by the author, user global rules present. The 1.3.0 safeguards restored in the final text were not separately re-tested.

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
