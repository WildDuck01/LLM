# 🚀 Lab 1 实验总结：LLM 对话摘要与 Prompt Engineering

---

## 🧠 一、实验目标

本实验旨在系统探索：

> **在不进行模型训练的情况下，通过 Prompt Engineering 与推理策略优化大语言模型（LLM）的对话摘要能力。**

具体包括：

- 使用 FLAN-T5 模型完成对话摘要任务
- 分析不同 Prompt 设计对模型输出的影响
- 探索 Zero-shot / One-shot / Few-shot 推理效果
- 研究推理参数（如 temperature）对生成结果的影响

---

## 📦 二、实验环境与工具

- 模型：`google/flan-t5-base`
- 数据集：`DialogSum`
- 框架：
  - PyTorch
  - Hugging Face Transformers
  - Hugging Face Datasets

---

## 🧩 三、实验流程

### 1️⃣ 数据加载与任务定义

任务形式：

- 输入：dialogue（对话）
- 输出：summary（摘要）

数据集中提供了人工标注的摘要作为 baseline。

---

### 2️⃣ Baseline（无 Prompt）

直接输入：

```text
dialogue
```

结果：

- 输出不完整
- 信息缺失严重
- 无法形成有效摘要

📌 结论：

> 模型无法自动理解任务，必须通过 Prompt 指定任务

---

### 3️⃣ Zero-shot + Instruction Prompt

示例：

```text
Summarize the following conversation.

[dialogue]

Summary:
```

效果：

- 明显优于 baseline
- 输出更接近摘要形式

📌 结论：

> Prompt 能激活模型的任务能力

---

### 4️⃣ Prompt Engineering 对比

不同 Prompt：

- “Summarize the following conversation”
- “What was going on?”

结果：

- 指令型 Prompt 更稳定
- 问句型 Prompt 输出不稳定

📌 结论：

> Prompt 本质是任务定义，不同表达影响模型行为

---

### 5️⃣ One-shot Learning

加入一个示例：

```text
Example:
[dialogue]
[summary]

New dialogue:
```

效果：

- 输出显著改善
- 更贴近人类摘要

📌 结论：

> 示例可以引导模型学习输出格式

---

### 6️⃣ Few-shot Learning

加入多个示例：

效果：

- 有一定提升
- 超过 5 个示例后收益有限

📌 结论：

> Few-shot ≠ 越多越好，存在边际递减

---

### 7️⃣ 推理参数实验

重点参数：

- temperature

表现：

| 参数 | 效果 |
|------|------|
| 低温（0.1） | 保守、稳定 |
| 中等（0.7） | 平衡 |
| 高温（1~2） | 随机性强 |

📌 结论：

> temperature 控制输出的随机性，而非正确性

---

## 🔥 四、核心结论

### 1. Prompt 决定任务

> 模型必须通过 Prompt 才能理解任务

---

### 2. Prompt 形式影响输出

> 不同表达方式会导致不同结果

---

### 3. 示例提升能力

> One-shot / Few-shot 可以显著增强效果

---

### 4. 参数控制输出风格

> temperature 等参数影响生成多样性

---

## 🧠 五、本质理解

LLM 推理过程可以抽象为：

```
Prompt → Tokenization → Model → Sampling → Output
```

其中：

- Prompt：定义任务
- Model：理解并生成
- Sampling：控制输出策略

---

## 🧾 六、总结

本实验验证了：

> **通过 Prompt Engineering 与推理策略优化，可以在不训练模型的情况下显著提升 LLM 表现。**

---

## 🔭 七、后续研究方向

- Sampling 机制深入理解
- Beam Search vs Greedy
- KV Cache 推理优化
- 模型压缩与加速
