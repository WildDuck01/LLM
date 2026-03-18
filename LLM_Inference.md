# LLM 推理机制与生成控制参数

本节系统总结大语言模型（LLM）在推理阶段（Inference）的核心概念，包括输入输出形式、In-Context Learning 以及生成控制参数。

---

## 1. 基本概念

在使用 LLM 时，可以将流程抽象为：

Prompt → Inference → Completion

| 概念 | 定义 |
|------|------|
| Prompt | 输入给模型的文本（问题 / 指令 / 上下文） |
| Inference | 模型根据 Prompt 进行计算和生成的过程 |
| Completion | 模型最终生成的输出内容 |

---

## 2. In-Context Learning（ICL）

In-Context Learning 指：

不更新模型参数，仅通过 Prompt 提供示例来引导模型行为

### 常见形式

| 类型 | 说明 |
|------|------|
| Zero-shot | 不提供示例，直接提问 |
| One-shot | 提供一个示例 |
| Few-shot | 提供多个示例 |

---

### 示例

#### Zero-shot

Translate to French: Hello

#### One-shot

English: Hello → French: Bonjour  
English: Good morning → French:

---

### 使用建议

- 简单任务：Zero-shot 即可
- 复杂任务：Few-shot 更稳定
- 高精度任务：考虑 Fine-tuning（微调）

---

## 3. 推理参数（Inference Configuration）

这些参数仅作用于推理阶段，不影响模型训练。

---

## 3.1 Sampling 机制（核心）

LLM 在生成每个 token 时，本质是：

logits → softmax → 概率分布 → 选择下一个 token

---

### 1️⃣ Greedy（贪心）

argmax(P(token))

- 每一步选择概率最大的 token
- 优点：稳定
- 缺点：容易重复、缺乏多样性

---

### 2️⃣ Random Sampling（随机采样）

- 按概率分布随机选择 token
- 优点：更有创造性
- 缺点：可能不稳定

---

## 3.2 Top-k Sampling

从概率最高的 k 个 token 中进行采样

- 限制候选范围
- 避免低概率噪声

---

## 3.3 Top-p Sampling（Nucleus Sampling）

选择累计概率 ≥ p 的最小 token 集合

- 动态调整候选数量
- 比 Top-k 更灵活

---

## 3.4 Temperature（温度）

用于控制 softmax 分布形状：

| Temperature | 效果 |
|------------|------|
| 0 | 完全确定（接近 greedy） |
| <1 | 更稳定 |
| =1 | 原始分布 |
| >1 | 更随机 |

本质：

- 低温 → 分布更尖锐（确定性强）
- 高温 → 分布更平滑（随机性强）

---

## 4. Max New Tokens（生成长度控制）

### 定义

Max New Tokens 表示模型最多可以生成的“新 token 数量”（不包含 Prompt）

---

### 形式化理解

Prompt 长度 = N  
Max New Tokens = M  

总长度 ≤ N + M

---

### 作用

#### 1️⃣ 防止无限生成

LLM 本质是“不断预测下一个 token”的过程：

没有限制 → 可能一直生成

Max New Tokens = 强制停止条件

---

#### 2️⃣ 控制推理成本

token 数量 ↑ → 推理时间 ↑ → 成本 ↑

Max New Tokens = 成本上限控制器

---

#### 3️⃣ 控制输出长度

| 场景 | 推荐长度 |
|------|----------|
| 问答 | 50 ~ 150 |
| 对话 | 100 ~ 300 |
| 代码生成 | 200 ~ 500 |
| 长文本生成 | 500+ |

---

## 5. 停止机制（Stop Condition）

模型生成停止有两种方式：

| 方式 | 说明 |
|------|------|
| EOS Token | 模型主动结束 |
| Max New Tokens | 达到上限强制结束 |

规则：

谁先触发，谁生效

---

## 6. 常见参数组合

| 场景 | 推荐配置 |
|------|----------|
| 精准问答 | low temperature + greedy |
| 对话系统 | temperature ≈ 0.7 + top-p |
| 代码生成 | temperature 0.2~0.5 |
| 创意写作 | high temperature + top-p |

---

## 7. 核心总结

LLM 推理的本质是：

在每一步生成中，从概率分布中选择下一个 token

不同参数控制的是：

- 是否随机（temperature）
- 随机范围（top-k / top-p）
- 生成长度（max_new_tokens）
- 停止条件（EOS / Max Token）

---

## 8. 当前理解进度

- [x] Prompt / Inference / Completion 概念  
- [x] In-Context Learning（Zero/Few-shot）  
- [x] Sampling（greedy / random）  
- [x] Top-k / Top-p  
- [x] Temperature  
- [x] Max New Tokens  
- [ ] Sampling 代码实现  
- [ ] 概率分布（softmax）深入理解  

---

## 9. 后续学习方向

- Sampling 的代码实现（logits → softmax → sampling）
- Beam Search vs Greedy
- KV Cache（推理加速）
- Speculative Decoding
- 推理优化与模型压缩
