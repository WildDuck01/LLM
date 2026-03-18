# 🚀 LLM 推理机制与生成控制参数

> 📌 本文系统总结 LLM 在推理阶段（Inference）的核心机制与关键参数控制方式  
> 👉 适用于：Transformer / LLM 入门 → 推理优化 → 论文基础

---

# 🧠 1. 基本概念

👉 Prompt → Inference → Completion

| 概念 | 定义 |
|------|------|
| **Prompt** | 输入给模型的文本（问题 / 指令 / 上下文） |
| **Inference** | 模型根据 Prompt 进行计算与生成的过程 |
| **Completion** | 模型最终生成的输出 |

---

# 🎯 2. In-Context Learning（ICL）

> ✅ 不更新模型参数，仅通过 Prompt 示例引导模型行为

---

## 📌 常见形式

| 类型 | 说明 |
|------|------|
| **Zero-shot** | 不提供示例 |
| **One-shot** | 提供一个示例 |
| **Few-shot** | 提供多个示例 |

---

## 📎 示例

### Zero-shot

👉 Translate to French: Hello

### One-shot

👉  
English: Hello → French: Bonjour  
English: Good morning → French:

---

## 💡 使用建议

- 简单任务 → Zero-shot  
- 复杂任务 → Few-shot  
- 高精度任务 → Fine-tuning  

---

# ⚙️ 3. 推理参数（Inference Configuration）

> 🚨 所有参数 **仅作用于推理阶段，不影响训练**

---

# 🎲 3.1 Sampling 机制（核心）

👉 logits → softmax → probability → sampling → next token

---

## ① Greedy（贪心）

👉 `argmax(P(token))`

- ✅ 稳定
- ❌ 容易重复

---

## ② Random Sampling（随机采样）

- 按概率分布随机选择 token  
- 更有创造性，但不稳定  

---

<details>
<summary>📦 Sampling 方法对比（点击展开）</summary>

| 方法 | 特点 |
|------|------|
| Greedy | 最稳定，但无多样性 |
| Random | 多样性强，但不稳定 |
| Top-k | 限制候选范围 |
| Top-p | 动态概率控制 |

</details>

---

# 🔢 3.2 Top-k Sampling

👉 从概率最高的 k 个 token 中采样

- 限制候选空间  
- 避免低概率噪声  

---

# 📊 3.3 Top-p Sampling（Nucleus）

👉 选择累计概率 ≥ p 的最小集合

- 动态 token 数量  
- 更自然  

---

# 🌡️ 3.4 Temperature（温度）

| Temperature | 效果 |
|------------|------|
| 0（极限） | 接近 greedy（one-hot 分布） |
| <1 | 更稳定（低随机性） |
| =1 | 原始 softmax 分布 |
| >1 | 更随机（高多样性） |

---

> 🎯 本质：
>
> - 低温 → 放大 logits 差异 → 分布更尖锐（确定性强）  
> - 高温 → 缩小 logits 差异 → 分布更平滑（随机性强）

---

# 📏 4. Max New Tokens（生成长度控制）

> ✅ 控制模型最多生成多少“新 token”（不包含 Prompt）

---

## 📐 数学理解

👉 Prompt = N tokens  
👉 Max New Tokens = M  
👉 总长度 ≤ N + M  

---

## 🎯 核心作用

### ① 防止无限生成

👉 LLM 本质是不断预测下一个 token 的系统  

---

### ② 控制推理成本

👉 token ↑ → latency ↑ → cost ↑  

---

### ③ 控制输出长度

| 场景 | 推荐 |
|------|------|
| 问答 | 50~150 |
| 对话 | 100~300 |
| 代码 | 200~500 |
| 长文本 | 500+ |

---

# 🛑 5. 停止机制（Stop Condition）

| 方式 | 说明 |
|------|------|
| **EOS Token** | 模型主动结束 |
| **Max New Tokens** | 强制停止 |

---

> ⚠️ 谁先触发，谁生效

---

# ⚡ 6. 参数组合策略

| 场景 | 推荐配置 |
|------|----------|
| 精准问答 | low temperature + greedy |
| 对话系统 | temperature ≈ 0.7 + top-p |
| 代码生成 | temperature 0.2~0.5 |
| 创意写作 | high temperature + top-p |

---

# 🧾 7. 核心总结

> 💡 LLM 本质是一个“token 生成系统”

---

👉 每一步做的事情：

1. 计算概率分布  
2. 采样 token  
3. 拼接输入  
4. 继续生成  

---

## 🔑 参数控制本质

- 🎲 是否随机 → `temperature`
- 🎯 随机范围 → `top-k / top-p`
- 📏 生成长度 → `max_new_tokens`
- 🛑 停止条件 → `EOS / Max Token`

---

# 📊 8. 当前理解进度

- [x] Prompt / Inference / Completion  
- [x] In-Context Learning  
- [x] Sampling（greedy / random）  
- [x] Top-k / Top-p  
- [x] Temperature  
- [x] Max New Tokens  
- [ ] Sampling 代码实现  
- [ ] Softmax 深入理解  

---

# 🔭 9. 后续学习方向

- KV Cache（推理加速）  
- Speculative Decoding  
- 推理优化 & 模型压缩  
- Sampling 代码实现（PyTorch）  
- Beam Search vs Greedy  

---

# 🧩 附：一句话理解

> 🧠 LLM = 一个不会停止的“续写机器”  
> 📏 Max New Tokens = 限制它最多写多少内容
