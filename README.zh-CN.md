# prompt-refiner

[English](README.md) | [简体中文](README.zh-CN.md)

`prompt-refiner` 是一个 Codex 技能，用于把粗略、零散或存在冲突的意图整理成可复用的 Prompt，或可直接执行的决策型计划。

它不只是润色文字。这个技能会在内部对模糊请求进行自问自答，保留用户的具体约束，为常规歧义选择安全默认值，只暴露真正重要的决策，并把请求路由到正确结果：输出 Prompt、准备并执行任务、直接回答只读问题，或进行一次简短澄清。

## 为什么使用

很多粗略请求都隐藏着决定任务成败的关键条件，例如目标用户、范围、非目标、信息时效、权限、输出格式、验证方式和回滚方案。`prompt-refiner` 会把这些条件补齐，同时避免让用户陷入冗长问答。

核心行为：

- 普通请求通常在内部压测约 `20-35` 个问题，复杂任务约 `35-50` 个。
- 只有在可见决策记录确实有帮助时，才输出精炼后的 `8-15` 条摘要。
- 常规歧义由技能选择推荐默认值，最多只询问 `1-3` 个高影响问题。
- 保留用户提供的模板、规则、原始措辞和硬性约束。
- 非简单直接执行任务会先创建 `plan.md`。
- 涉及时效性事实时要求最新核实，并区分事实与推断。
- 将“展示完整思考链”改写为可审计的决策摘要、证据和验证结果。

## 路由模型

| 用户意图 | 技能行为 |
| --- | --- |
| 完善、改写、比较或压测 Prompt | 返回可直接复制的完善版 Prompt |
| 执行一个非简单任务 | 梳理决策、创建 `plan.md`，然后执行 |
| 询问稳定的只读问题 | 直接回答，不强制输出 Prompt 框架 |
| 请求较模糊 | 推断最安全且有用的路径，必要时简短澄清一次 |
| 明确说 `纯prompt` 或 `只要prompt` | 只返回一个 Prompt 代码块 |
| 明确说 `不要问` 或 `直接继续` | 内部解决可安全默认的歧义并继续执行 |

## 安装

克隆仓库：

```bash
git clone https://github.com/kyoka-shuiyue/prompt-refiner.git
cd prompt-refiner
```

将技能目录复制到 Codex 技能目录：

```bash
mkdir -p ~/.codex/skills/prompt-refiner
cp -R prompt-refiner/. ~/.codex/skills/prompt-refiner/
```

Windows PowerShell：

```powershell
$target = "$env:USERPROFILE\.codex\skills\prompt-refiner"
New-Item -ItemType Directory -Force $target | Out-Null
Copy-Item -Recurse -Force .\prompt-refiner\* $target
```

如果当前环境不会自动热重载技能，请重启 Codex。

## 更新

拉取仓库最新内容，然后重新复制技能目录：

```bash
git pull --ff-only
mkdir -p ~/.codex/skills/prompt-refiner
cp -R prompt-refiner/. ~/.codex/skills/prompt-refiner/
```

Windows PowerShell：

```powershell
git pull --ff-only
$target = "$env:USERPROFILE\.codex\skills\prompt-refiner"
New-Item -ItemType Directory -Force $target | Out-Null
Copy-Item -Recurse -Force .\prompt-refiner\* $target
```

## 使用方法

需要时可以显式调用技能：

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
$prompt-refiner 不要问，先自问自答，再在当前仓库完成这个功能。
```

技能的 frontmatter 描述有意覆盖模糊请求和非简单任务，因此即使没有显式写出 `$prompt-refiner`，Codex 也可能根据请求自动加载它。

## 工作流程

1. 判断请求属于 Prompt 输出、直接执行、直接回答还是意图模糊。
2. 在内部检查目标、成功标准、范围、约束、风险和验证方式。
3. 用推荐默认值解决低风险歧义。
4. 只暴露高影响争议或真正阻塞项。
5. 产出并压测 Prompt、答案或执行计划。
6. 对非简单直接执行任务，在实现前保存 `plan.md`。

## 仓库结构

```text
prompt-refiner/
  SKILL.md                  核心路由与完善规则
  agents/
    openai.yaml             Codex 界面元数据
  references/
    patterns.md             进阶处理模式
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

## 评测

手动评测集覆盖简单改写、复杂工程 Prompt、直接执行、纯 Prompt 覆盖规则、禁止追问、时效信息、高风险请求、技术冲突、候选 Prompt 合并和隐藏思考请求。

评测步骤：

1. 安装待测版本的技能。
2. 在全新线程或代理上下文中逐条运行 `evals/prompt-refiner-cases.jsonl`。
3. 使用 [evals/rubric.zh-CN.md](evals/rubric.zh-CN.md) 评分。
4. 将独立测试结果记录到前向测试报告。

不要把预期答案交给被评测代理，只提供技能路径和测试请求。

## 自定义与学习块

`SKILL.md` 末尾的 `SKILLOPT-SLEEP:LEARNED` 区块用于保存经过验证的本地偏好。通用行为应放在该区块之前，机器维护的偏好应保留在区块内部。公开发布分支前应检查学习条目，因为其中可能包含个人工作流偏好。

## 设计原则

- 可执行的清晰度优先于装饰性冗长。
- 安全默认值优先于不必要的追问。
- 用户约束优先于自行发明需求。
- 可见证据优先于隐藏思考链。
- 最新一手来源优先于过时假设。
- 可测试验收标准优先于模糊质量描述。

## 许可证

MIT，详见 [LICENSE](LICENSE)。
