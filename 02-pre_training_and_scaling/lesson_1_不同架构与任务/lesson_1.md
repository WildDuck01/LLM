# 📘 LLM 模型架构与预训练机制（精炼总结）

---

## 2️⃣ 本课主线：核心逻辑

> **模型架构 → 预训练目标 → 模型能力 → 适用任务**

```
Encoder-only      → MLM  → 理解能力强
Decoder-only      → CLM  → 生成能力强
Encoder-Decoder   → Span → 输入→输出映射能力
```

---

## 3️⃣ 预训练（Pre-training）

### 🧠 本质
- 从海量文本中学习语言规律
- 使用 **自监督学习（Self-supervised）**

### 📊 数据与计算
- 数据规模：GB / TB / PB
- 需要 GPU 大规模训练

### ⚠️ 关键现实
- 数据需清洗（去噪 / 去偏 / 过滤）
- 实际可用数据仅：

> **≈ 1% – 3%**

---

## 4️⃣ 三大模型架构

---

### 🔵 4.1 Encoder-only（Autoencoding）

```
输入:  The teacher <MASK> the student
输出:  teaches
```

- 🎯 目标：MLM（掩码预测）
- 🔁 特性：双向上下文（Bidirectional）

#### ✅ 适用任务
- 情感分析
- NER
- 分类

#### 🧩 代表模型
- BERT
- RoBERTa

> 💡 **关键词：理解文本**

---

### 🟡 4.2 Decoder-only（Autoregressive）

```
输入:  The teacher
输出:  teaches → the → student
```

- 🎯 目标：CLM（预测下一个token）
- ➡️ 特性：单向上下文（Unidirectional）

#### ✅ 适用任务
- 文本生成
- 对话系统
- 写作

#### 🧩 代表模型
- GPT
- BLOOM

> 💡 **关键词：生成文本**

---

### 🟢 4.3 Encoder-Decoder（Seq2Seq）

```
输入:  The teacher <X> student
输出:  <X> teaches the
```

- 🎯 目标：Span Corruption
- 🔁 特性：理解 + 生成

#### ✅ 适用任务
- 翻译
- 摘要
- 问答

#### 🧩 代表模型
- T5
- BART

> 💡 **关键词：输入 → 输出**

---

## 5️⃣ 模型对比一览表

| 架构 | 训练目标 | 上下文 | 能力 | 场景 |
|------|----------|--------|------|------|
| Encoder-only | MLM | 双向 | 理解 | 分类 / NER |
| Decoder-only | CLM | 单向 | 生成 | 对话 / 写作 |
| Encoder-Decoder | Span | 混合 | 转换 | 翻译 / 摘要 |

---

## 6️⃣ 模型选型逻辑

### 🧭 快速判断

| 任务类型 | 推荐架构 |
|----------|----------|
| 理解文本 | Encoder-only |
| 生成文本 | Decoder-only |
| 文本转换 | Encoder-Decoder |

---

## 🔟 核心结论

- ✔ 模型选择 ≠ 看谁更大
- ✔ 本质是：**任务 → 能力 → 架构**
- ✔ 三类模型分工明确：

```
理解 → Encoder
生成 → Decoder
转换 → Encoder-Decoder
```

- ✔ 大模型更强，但成本极高

---

## 1️⃣2️⃣ 学习本课的正确方式

### 🎯 建立核心映射

```
任务 → 能力 → 架构 → 训练目标
```

### 🧪 示例

| 任务 | 架构 | 原因 |
|------|------|------|
| 情感分析 | Encoder | 双向理解 |
| 对话生成 | Decoder | 连续生成 |
| 文本摘要 | Enc-Dec | 输入→输出 |

---

## 📌 一页速记

```
Encoder-only  = 理解
Decoder-only  = 生成
Enc-Dec       = 转换
```

> 🚀 **先看任务，再选模型！**
