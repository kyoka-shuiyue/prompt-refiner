# prompt-refiner

[English](README.md) | [简体中文](README.zh-CN.md)

`prompt-refiner` 是一个模型无关的任务合同完善技能，用来把粗略、零散或存在冲突的意图整理成清晰的任务合同。它帮助聪明的 AI 模型理解终点和护栏，同时保留自主选择最佳路径的空间。

它不是 Prompt 加长器，也不是固定问卷。它只补全真正影响任务的五类信息：

1. **目标**：用户最终能看到或获得什么。
2. **成功标准**：任务完成时必须满足什么。
3. **重要边界**：范围、事实、权限、安全、证据、成本和兼容性约束。
4. **停止条件**：何时完成、重试、降级、询问、拒绝判断或停止。
5. **必要澄清**：哪些未决选择会改变前四项。

工具、模块、工作流、输出结构和实现步骤都是辅助信息。只有它们会改变任务合同，或用户明确要求时，才应该加入。

## 为什么这样设计

模型越聪明，Prompt 越不需要替它规定每一步，更需要把正确目标、证据标准、重要边界和停止行为讲清楚。因此，`prompt-refiner` 追求的是“最小充分任务合同”：

- 把终点说清楚，但不替模型规定每条路；
- 对低影响歧义使用安全默认值；
- 只询问答案会实质改变任务的问题；
- 保留用户给出的事实、结构、意图和授权边界；
- 对时效性事实要求最新证据；
- 让完成条件与失败处理可以被观察和验证；
- 避免自行发明需求和堆砌流程。

它没有固定的内部提问数量、可见摘要数量或强制计划文件。完善深度由任务的风险、复杂度和决策影响决定。

## 路由行为

| 用户意图 | 技能行为 |
| --- | --- |
| 完善、改写、比较或压测 Prompt | 返回可直接复制的完善版 Prompt |
| 执行底层任务 | 澄清关键决策后，在授权范围内执行 |
| 询问只读问题 | 直接回答，不强制套 Prompt 框架 |
| 请求存在歧义 | 先安全检查，推断低风险默认值，必要时集中澄清一次 |
| 明确说 `纯prompt` 或 `只要prompt` | 只返回一个 Prompt 代码块 |
| 明确说 `不要问` 或 `直接继续` | 内部解决可安全默认的歧义并继续 |

## 安装

克隆仓库：

```bash
git clone https://github.com/kyoka-shuiyue/prompt-refiner.git
cd prompt-refiner
```

把唯一的 `prompt-refiner/` 技能目录复制到你的 AI Agent 或技能宿主所要求的目录：

```bash
cp -R prompt-refiner /path/to/your-agent/skills/
```

Windows PowerShell：

```powershell
$skillsRoot = "C:\path\to\your-agent\skills"
Copy-Item -Recurse -Force .\prompt-refiner (Join-Path $skillsRoot "prompt-refiner")
```

如果宿主不会自动检测技能，请重新加载或重启。宿主需要支持 `SKILL.md` 风格的技能；准确目录和调用语法以对应宿主文档为准。

更新时，在本仓库运行 `git pull --ff-only`，然后重新复制这个唯一技能目录。

## 使用方法

显式调用：

```text
使用 $prompt-refiner 完善这个请求：
做一个可以检索本地文件的知识库工具。
```

只输出 Prompt：

```text
$prompt-refiner 纯prompt：完善这个本地 AI 桌面工具的开发需求。
```

使用安全默认值并直接执行：

```text
$prompt-refiner 不要问，使用安全默认值，在当前仓库完成这个功能并验证结果。
```

支持隐式技能路由的宿主，也可以根据 frontmatter 中的描述自动加载该技能。

## 工作方式

1. 先判断用户要的是 Prompt、直接执行、只读回答还是澄清。
2. 围绕五项任务合同字段进行内部压测，不设置固定问题数量。
3. 对低影响歧义选择合理默认值。
4. 只有答案会改变范围、权限、安全、证据、成本或验收标准时，才进行最多一轮、`1-3` 个集中问题的澄清。
5. 输出最小充分的 Prompt、答案或执行合同。
6. 检查完成标准、授权、信息时效、停止行为、规则冲突和不必要的路径规定。

只有真正复杂、多阶段且需要恢复上下文的执行任务，才使用持久计划。明确的局部修改、只读回答和纯 Prompt 工作保持轻量。

## 仓库结构

```text
prompt-refiner/
  SKILL.md                  唯一的模型无关技能
  agents/
    openai.yaml             兼容宿主可选使用的界面元数据
  references/
    patterns.md             特殊情况处理模式
evals/
  prompt-refiner-cases.jsonl
  rubric.md
  rubric.zh-CN.md
  forward-test-report.md
  forward-test-report.zh-CN.md
README.md
README.zh-CN.md
LICENSE
```

本仓库只维护一个规范版本。`agents/` 下的文件是同一技能的宿主界面元数据，不是不同模型或客户端的独立版本。

## 评测

手动评测集覆盖明确改写、模糊产品需求、时效信息、模糊代码修改、授权明确的局部实现、破坏性操作、高风险判断、隐藏思考请求、技术冲突、输出覆盖、结构保留和授权边界。

评测步骤：

1. 在全新的 Agent 上下文中加载待测技能。
2. 逐条运行 [evals/prompt-refiner-cases.jsonl](evals/prompt-refiner-cases.jsonl) 中的请求，不向被测 Agent 展示预期行为。
3. 使用 [evals/rubric.zh-CN.md](evals/rubric.zh-CN.md) 评分。
4. 在前向测试报告中记录准确版本、方法、限制和结果。

量表评估目标清晰度、成功标准、重要边界、停止条件、澄清纪律、路径自主性和信息密度，不奖励冗长内容或隐藏思考。

## 设计原则

- 任务合同优先于穷举规格。
- 可观察的成功标准优先于模糊质量要求。
- 安全默认值优先于不必要的追问。
- 重要边界优先于规定实现路径。
- 最新证据优先于过时假设。
- 可见决策和验证优先于隐藏思考链。
- 任务合同充分后立即停止扩写。

## 许可证

MIT，详见 [LICENSE](LICENSE)。
