# Prompt 模板库（可直接复制）

所有模板保留英文原文——它们将原样喂给 AI 代理。`[方括号]` 为需替换的占位内容。执行对应技术前先取用本库模板。

## 目录

1. [T1] Seed String of Thought（随机字符串种子）
2. [T2] 大胆 prompt 三则示例
3. [T2] 想法发散与打磨三步系统
4. [T3] 设计评论家循环主 prompt（含评论家子代理指引）
5. [T4] 图像生成增强
6. [T4] API key 安全接入说明
7. [T5] 视频动画图形（抠像叠加）
8. [T5] 视频状态转场（滚动驱动）
9. [T6] 减法打磨指令

---

## 1. [T1] Seed String of Thought（随机字符串种子）

随机字符串由代理在 shell 中生成（真随机），模型只负责从字符串中“读出”灵感。字符串不得出现在成品中。

```text
I want you to build me a landing page for my [产品描述，如 productivity app].

Follow this procedure:

1. Generate a long, random alphanumeric string using a shell script.

2. Define the creative direction (color scheme, layout, typography, etc.) based on the string. Look beyond the surface for subpatterns, special numbers, anything that inspires you.

3. Use your judgment to bring this direction to life and make it look great.

Don't reveal the string in the design. It's only for your inspiration.
```

**为什么有效：** 模型无法真随机，只能预测“听起来随机”的 token；外部生成的字符串是模型训练分布之外的输入，能迫使它做出真正不同的决策。每次运行结果唯一。

## 2. [T2] 大胆 prompt 三则示例

灵感注入法：把具体灵感（游戏、城市、艺术运动）写进 prompt，让模型围绕它做设计决策，而不是临场发挥。

```text
Build me a landing page for my productivity app, with a bold pixel art theme and stunning graphics. Each section should feel like a still from a video game, yet somehow it should all function as a landing page.
```

```text
Build me a landing page for my productivity app, set in an isometric living 3D city, where different features are somehow represented by neighborhoods or buildings.
```

```text
Build me a landing page for my productivity app, with a radically asymmetric layout, dissonant colors and typography, and uncomfortable negative space. Break all the rules but still make it look good.
```

## 3. [T2] 想法发散与打磨三步系统

**第 1 步 · 发散（故意不给细节，只激发想象力）：**

```text
I want to come up with a bold, unique design language for my product. Can you list as many ideas as you can, with short, high-level descriptions? Go broad, not deep.
```

**第 2 步 · 收敛（诚实反馈 + 让 AI 按你的口味打磨）：** 从列表中挑一个方向，写下你对它的真实反应——什么感觉对、什么廉价/无聊/过火、缺什么。参考格式：

```text
[方向名，如 Industrial Control Panel]:

- I'm imagining something tactile. Clicky, satisfying buttons, nice sounds.
- Initially I pictured something cartoony or skeuomorphic, but this feels tacky to me. Avoid that.
- Instead, want consistent components and little touches that land this look without going overboard.
- Gray gradients would look boring. Need more texture. Maybe we can incorporate some color, while retaining the control panel feel?

Can you sharpen this one based on my tastes?
```

**第 3 步 · 成文（满意后让 AI 写出可执行的构建 prompt）：**

```text
Can you write a concise prompt that an AI agent could use to build an initial POC page with this?
```

**心法：** 直接把 AI 生成的想法回喂给 AI，只能得到人人都能得到的东西；只有你主动注入品味，产物才只属于你。敢于尝试听起来很烂的方向。

## 4. [T3] 设计评论家循环主 prompt

编码代理执行、评论家代理评审。整体结构：截图 → 全新上下文调评论家 → 反馈 → 修改 → 重复，直到评论家独立评分 ≥9/10。

```text
I want you to improve this design. To figure out what to focus on, use a [评论家模型名，如 Fable 5] subagent as a design critic.

Follow this procedure at each iteration:

- Capture a screenshot of the current design

- Invoke the critic in a fresh context, with just the screenshot, not the code, implementation details, or earlier iterations/critiques

- Ask it to evaluate the aesthetic that the design is going for, imagine how a top design studio would execute this aesthetic, then outline the biggest gaps

- Lastly, it should provide a score out of 10 indicating how close the current design is to that studio-level quality bar

Provide this guidance to the critic in its prompt:

- It should think high-level about the overall structure and composition as well as look at the fine details

- It should watch out for patterns that feel overdone, excessive, or otherwise obviously AI-generated, and penalize them

- It should provide tight, specific feedback, not vague prose

- It should be bold and opinionated, not rely on what's safe or easy

Your work is only complete when the critic independently deems it 9/10 or higher. Do not put that criterion in the critic prompt; keep it objective in its scoring. Use the same critic prompt each time.
```

**要点：** “≥9/10”是停止条件，只能写在执行代理的 prompt 里，不能写进评论家 prompt——否则评论家会自我修正评分。每轮评论家 prompt 保持一致。

**更强客观性（可替换上面第 3-4 条步骤）：**

```text
Here are 5 designs: 4 professional examples and 1 screenshot of our product. Rank them by polish and taste level.
```

## 5. [T4] 图像生成增强

```text
The design is pretty plain. Add more personality using image generation. Consider shaders or 3D effects in combination with images to create more interesting visuals.

For image generation, use this OpenAI API key (only use it locally, do not store it in the code or product): [单独限额的 sk-...]

Verify that your work looks right frame-by-frame in the browser.
```

## 6. [T4] API key 安全接入说明

- 为代理单独创建一个 key，**设置严格消费限额**；即使泄露或被滥用，成本可控、可随时吊销。
- 频繁粘贴时改存文件：`Create a gitignored file called .env.agents, store this API key in it, and note to yourself in AGENTS.md/CLAUDE.md that these keys are for you to use during development (but must not ship with the product).`
- 有订阅时优先走订阅计费（如 Codex CLI 记账），不产生 API 费用。

## 7. [T5] 视频动画图形（抠像叠加）

流程：纯色背景循环视频 → 对着页面背景色先渲染（烘进折射/光影效果）→ 色度键或视频 matting 去背景 → 叠加进 UI。

```text
Can you replace the image on this page with a looping video clip that does something more interesting? Have the [主体，如 crystal] splinter apart and slowly spin around. It should have awesome glassy effects that refract the page background and cast shadows and light around it.

To get convincing glass refraction effects, render the video of the glass over the page background colors first (so it bakes in the refraction effects), then remove the background with a video matting model.

Use this fal.ai API key: [sk-...]

Find appropriate recent models for video generation and background removal.
```

## 8. [T5] 视频状态转场（滚动驱动）

流程：图像生成首帧 → 视频模型从该帧动画到下一状态 → 用上一段最后一帧 seed 下一段 → 无缝衔接 → 随滚动/滑动逐帧 scrubbing。

```text
Build a demo page for a [产品，如 suitcase] that uses a video model to create interactive transitions between a couple of screens. Each screen should show the [产品] in a different state, with vertical motion that feels appropriate for scrolling:

- Initially, have the [产品] floating high up in the air

- Then have it land on the floor and pop open

- Finally, have its contents neatly land into it from the top

Generate the initial frame using your image generation skill. Then, generate a video clip that starts from that frame and animates to the next state. Use the final frame of that video to seed the next transition so that it continues seamlessly. Scrub through the transitions one by one as the user scrolls.

Use this fal.ai API key: [sk-...]

Use a video model with strong physics and consistency, like Seedance 2.5.
```

## 9. [T6] 减法打磨指令

在初稿（或评论家循环后）批量执行，直接指示收敛：

```text
- Simplify the layout into an image-centric grid

- Get rid of gradients, glows, and unnecessary containers

- Aim for a truly minimalist aesthetic that feels Apple-native
```

更通用的版本：`Look over the design and cut everything that doesn't add value: redundant labels, decorative highlights, non-native custom components, and over-explained copy. Let the visuals speak for themselves.`
