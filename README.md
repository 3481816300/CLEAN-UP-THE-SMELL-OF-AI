# doubao-ai-design-director

**把 AI 从"设计委员会式的中庸产出"提升为"世界级设计师"的方法论技能。**

> 基于 Anshu Chimala 发表于 Lenny's Newsletter 的《How to turn your AI into a world-class designer》(2026-09) 沉淀。这不是一份摘要，而是一套可直接执行的工作流：三阶段流程 × 7 项技术 × 可直接复制的 prompt 模板。

## 为什么需要它

LLM 是 next-token 预测器，训练使其倾向做出"让大多数人满意"的安全选择——这恰好是设计界的"委员会评审"：紫色渐变、左文右图、顶部导航、固定 CTA……**所有 AI 网站都长一样**。

伟大设计 = 情感 + 打破规则 + 令人难忘的意外选择，与 LLM 天性相反。因此多样性**必须从模型外部注入**：外部随机性、你的品味、外部评审者、外部工具。

## 方法总览

| 阶段 | 目标 | 核心技术 |
|---|---|---|
| **Discover** 发散 | 探索可能性的空间，先宽后深，摆脱默认模式 | T1 seed string 注入随机性 · T2 大胆 prompt + 想法系统 |
| **Define** 深化 | 给设计注入个体身份，逼近顶级工作室水准 | T3 评论家反馈循环 · T4 图像生成增强 · T5 视频生成动效 |
| **Deliver** 交付 | 打磨细节，删掉不增值元素，消除 AI 痕迹 | T6 减法打磨 · T7 消除 AI 痕迹 |

推荐路径：T2 想法系统定方向 → T1/T2 prompt 出初稿 → T3 评论家循环迭代到 ≥9/10 → T4/T5 按需加图加动效 → T6/T7 打磨交付。

## 安装

把本目录（含 `SKILL.md` 与 `references/`）复制到你的技能目录，例如：

```bash
# 豆包工作环境
cp -r doubao-ai-design-director ~/.doubao/agent_mode/workspace/.user_skills/

# 其他支持 SKILL.md 格式的 Agent（如 Claude Code / Codex）
cp -r doubao-ai-design-director ~/.claude/skills/
```

安装后，当任务命中描述中的触发场景（设计网页/落地页/UI、抱怨"AI 味/千篇一律"、要求"世界级/独特"设计、或对已有 AI 设计迭代打磨）时，技能会自动加载。

## 使用示例

- "帮我做一个不像 AI 生成的落地页"
- "用这套方法把我们的产品页打磨到顶级工作室水准"
- "给这个设计做评论家迭代，直到 9 分以上"

## 目录结构

```
doubao-ai-design-director/
├── SKILL.md                      # 主文档：三阶段流程 + 7 技术 + 跨阶段守则
└── references/
    ├── prompt-library.md         # 9 组可直接复制的英文 prompt 模板
    ├── critic-loop.md            # 设计评论家循环深度配置（标准分级/停止条件/模型分工）
    └── polish-checklist.md       # 交付前打磨清单（AI 痕迹/减法/验证）
```

## 致谢与来源

- 方法论与示例：Anshu Chimala《How to turn your AI into a world-class designer》，Lenny's Newsletter（付费文章）。其中 T7「消除 AI 痕迹」正文未公开（付费墙后），本技能中的对应清单系依据全文示例推断并已注明。
- 技术 1（Seed strings）：Sakana AI 发表的 String Seed of Thought。
- 本仓库为独立整理实现，与原文章作者及 Lenny's Newsletter 无隶属关系。

## License

MIT
