# 新鲜上下文前向测试报告

[English](forward-test-report.md) | [简体中文](forward-test-report.zh-CN.md)

日期：2026-06-27

## 测试设置

启动不带父线程上下文的新代理。每个代理只获得 `prompt-refiner` 技能路径和一条模拟用户请求。

被测技能路径：

```text
C:\Users\MK\.codex\skills\prompt-refiner
```

## 测试结果

| 用例 | 请求重点 | 结果 | 观察 |
| --- | --- | --- | --- |
| direct-task-boundary | 解释 frontmatter description 为什么影响触发；用户明确要求不要写 Prompt | 通过 | 代理直接回答，没有输出 `完善后的 Prompt` 或自问自答区块，验证了直接回答边界。 |
| conflict-hidden-current-info | 优化一个同时要求完整思考链、最新比赛研究、禁止联网且不要引用的 Prompt | 通过 | 代理识别了冲突，没有假装拥有最新资料，并将隐藏思考改为公开摘要，最终给出可用的离线 Prompt。 |
| vague-product-app | 完善记事应用 Prompt | 未运行 | 新代理立即遇到用量限制。 |
| vague-product-app-retry | 上述用例重试 | 未运行 | 重试仍遇到用量限制。 |

## 关键证据

`direct-task-boundary`：

- 回答说明 frontmatter `description` 是加载技能正文之前使用的触发摘要。
- 解释了过窄、过宽描述以及常见触发词的影响。
- 没有产出完善版 Prompt。

`conflict-hidden-current-info`：

- 回答指出最新研究需要联网或用户提供资料。
- 回答指出不应要求完整隐藏思考链。
- 请求被重写为离线预测 Prompt，包含信息边界、不确定性、禁止伪造来源和禁止确定性投注结论。

## 结论

两个已完成用例支持最重要的行为声明：

- `prompt-refiner` 不会再把直接执行或直接回答请求强行变成 Prompt 编写。
- 它能够正确处理隐藏思考与时效信息冲突。

产品 Prompt 用例仍需在新代理可用时重新独立测试。

## 2026-07-05 更新说明

此后技能加入了更深入的执行路径：非简单工作应使用 `20-50` 个内部自问问题，展示约 `8-15` 条精炼决策，只询问高影响争议，创建 `plan.md` 后再实现。该行为仍需要新的前向测试。

## 2026-07-10 更新说明

仓库此后同步了四路路由契约，以及明确的 `纯prompt` 和 `不要问` 覆盖规则。评测用例已经覆盖这些行为，但本次修订尚未完成新的独立前向测试。
