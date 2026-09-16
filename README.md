# AI Marketing Studio

> 面向跨境电商卖家的 AI 营销创意生成工具：输入产品信息并上传真实产品图片，自动完成产品分析、卖点提炼、广告文案生成、营销视觉生成，并支持人工修改 Prompt 后重新生成图片。

---

## 1. 项目简介

跨境电商卖家在制作商品营销内容时，通常需要在多个 AI 工具之间反复切换：

**分析产品 → 提炼卖点 → 写广告文案 → 编写图片 Prompt → 生成图片 → 调整 Prompt → 再次生成**

整个过程步骤较多，而且不同模型之间需要重复复制产品信息和 Prompt。

因此，我设计了 **AI Marketing Studio**，将这些步骤整合成一个完整的 AI Workflow。

用户只需要：

1. 输入产品名称、品类、目标受众和真实产品参数
2. 上传产品图片
3. 点击「生成营销创意」

系统即可自动输出：

- 目标受众分析
- 核心卖点
- 中文广告文案
- English Ad Copy
- AI 营销图片
- 可编辑的中文 Image Prompt

用户还可以修改 Image Prompt，再次生成营销图片，并下载最终结果。

---

## 2. 核心思路

这个项目并不是简单地：

> 输入一句话 → 调用一个大模型 → 返回结果

而是将不同 AI 模型拆分成不同角色，通过 Workflow 协同完成任务。

```text
用户输入产品信息
        ↓
上传真实产品图片
        ↓
Agnes Vision
识别产品视觉特征
        ↓
DeepSeek
分析目标受众 / 卖点 / 营销方向
        ↓
DeepSeek
生成广告文案 + Image Prompt
        ↓
DeepSeek
生成英文 Detection Label
        ↓
Grounding DINO
定位真实产品
        ↓
SAM
分割真实产品
        ↓
Agnes Image
生成营销背景
        ↓
Image Compositor
真实产品 + AI 背景合成
        ↓
最终营销图片
        ↓
用户审核 / 修改 Prompt
        ↓
重新生成 / 下载图片
```

核心原则是：

**LLM 负责理解与决策，视觉模型负责识别与生成，传统程序负责确定性处理。**

---

## 3. 为什么不直接让 AI 生成整张商品图？

在测试过程中，我发现直接使用图片生成模型生成完整商品广告图时，AI 容易修改真实产品的：

- 外观
- 比例
- Logo
- 接口
- 按键
- 产品结构

这对于电商场景是一个明显问题，因为营销图片中的商品应该尽可能保持真实。

因此，我将流程拆成：

```text
真实产品图片
      ↓
Grounding DINO 定位产品
      ↓
SAM 分割真实产品
      ↓
获得透明背景真实商品 PNG


AI Image Model
      ↓
只生成营销背景


真实商品 PNG + AI Background
      ↓
Image Compositor
      ↓
Final Marketing Image
```

这样可以将：

**“商品真实性”**

和

**“AI 创意背景生成”**

拆开处理。

---

## 4. AI Workflow

### Step 1 — Product Input

用户输入：

```text
Product Name
Category
Target Audience
Product Parameters
Product Image
```

例如：

```text
产品名称：Anker Power Bank

产品类别：移动电源

目标受众：
大学生、通勤上班族、旅行用户

产品参数：
20000mAh
支持快速充电
便携设计
```

---

### Step 2 — Product Visual Analysis

系统首先使用 **Agnes Vision** 分析上传的真实产品图片。

主要识别：

- 产品类型
- 外观
- 颜色
- 材质
- 结构
- 视觉特征

这些信息会作为后续 AI Workflow 的视觉上下文。

---

### Step 3 — Marketing Strategy

**DeepSeek** 根据：

```text
产品信息
+
真实产品参数
+
目标受众
+
产品视觉分析
```

生成结构化营销策略。

输出包括：

```json
{
  "target_audience": "目标受众",
  "selling_points": [
    "卖点 1",
    "卖点 2",
    "卖点 3"
  ],
  "ad_copy": {
    "zh": "中文广告文案",
    "en": "English advertising copy"
  },
  "marketing_direction": "营销方向"
}
```

其中：

- 用户输入和策略分析主要使用中文
- 广告文案同时生成中文和英文版本

这样更符合跨境电商营销场景。

---

### Step 4 — Detection Label Generation

用户输入的产品名称不一定适合目标检测。

例如用户可能只输入：

```text
Anker
```

但视觉检测模型更需要：

```text
portable power bank
```

因此 Workflow 会根据产品视觉分析结果，让 DeepSeek 自动生成一个简短的英文：

```text
Detection Label
```

再交给 Grounding DINO。

这一步将：

**用户的自然语言输入**

转换成：

**视觉模型更容易理解的检测语言。**

---

### Step 5 — Product Detection

使用 **Grounding DINO** 对真实产品进行目标检测。

```text
Product Image
      +
Detection Label
      ↓
Grounding DINO
      ↓
Bounding Box
```

系统会获得产品在图片中的位置。

---

### Step 6 — Product Segmentation

检测完成后，将 Bounding Box 传给 **SAM（Segment Anything Model）**。

```text
Bounding Box
      +
Original Image
      ↓
SAM
      ↓
Product Mask
```

最终生成：

```text
Transparent Product PNG
```

即保留真实商品、移除原始背景。

---

### Step 7 — AI Background Generation

**Agnes Image** 负责生成营销摄影背景。

当前 Workflow 中，Agnes 的主要职责不是重新生成商品，而是生成：

```text
EMPTY COMMERCIAL BACKGROUND
```

例如：

- 极简商业摄影背景
- 中性色调
- 柔和商业光线
- 干净商品台面
- 大面积留白
- 无人物
- 无 Logo
- 无额外商品

这样可以降低 AI 自动生成假商品、重复商品或奇怪物体的概率。

---

### Step 8 — Image Compositing

系统通过 `ImageCompositor` 将：

```text
真实商品透明 PNG
        +
AI 生成背景
        ↓
Image Compositor
        ↓
Final Marketing Image
```

最终形成完整的营销图片。

---

## 5. Human-in-the-loop

这个项目没有让 AI 完全自动决定最终结果，而是加入了 **Human-in-the-loop**。

第一次生成后，系统会展示：

```text
AI Image Prompt
```

用户可以直接修改中文 Prompt。

例如：

```text
原 Prompt
↓
高级极简商业摄影背景，米白色...

用户修改
↓
改成浅灰色背景，增加柔和侧光...
```

然后点击：

```text
重新生成图片
```

第二次生成时，不需要重新运行完整营销分析流程，而是复用：

```text
Original Product Image
+
Detection Label
+
Edited Image Prompt
```

重新执行图片生成流程。

这样既保留用户控制权，也减少不必要的 AI 调用。

---

## 6. 系统架构

```text
┌─────────────────────────────┐
│          Frontend           │
│ HTML / CSS / JavaScript     │
└──────────────┬──────────────┘
               │
               │ HTTP
               ▼
┌─────────────────────────────┐
│           FastAPI           │
│          backend            │
└──────────────┬──────────────┘
               │
               ▼
┌─────────────────────────────┐
│    AdGenerationWorkflow     │
│                             │
│ Workflow Orchestration      │
└──────────────┬──────────────┘
               │
      ┌────────┼─────────┐
      │        │         │
      ▼        ▼         ▼
 DeepSeek   Agnes     Vision Models
   LLM      Image
      │                  │
      │            Grounding DINO
      │                  │
      │                 SAM
      │                  │
      └────────┬─────────┘
               ▼
        ImageCompositor
               │
               ▼
       Final Marketing Image
```

---

## 7. 项目目录

```text
ai-product/
│
├── frontend/
│   ├── index.html
│   ├── style.css
│   └── app.js
│
├── backend/
│   ├── main.py
│   │
│   ├── workflow/
│   │   └── ad_generation.py
│   │
│   └── providers/
│       │
│       ├── llm/
│       │   ├── deepseek.py
│       │   └── mock.py
│       │
│       ├── vision/
│       │   └── agnes.py
│       │
│       ├── image/
│       │   └── agnes.py
│       │
│       ├── background/
│       │   ├── product_segmenter.py
│       │   └── remover.py
│       │
│       └── compositor/
│           └── image_compositor.py
│
├── test_images/
│
├── .env
├── .gitignore
└── requirements.txt
```

---

## 8. 各文件作用

### `frontend/index.html`

负责 Web 产品的页面结构。

主要包括：

```text
产品信息输入
产品图片上传
营销策略展示
广告文案展示
营销图片展示
Image Prompt 编辑
重新生成图片
下载营销图片
```

---

### `frontend/style.css`

负责整个 AI Marketing Studio 的 UI。

主要实现：

```text
Dashboard Layout
产品输入区
AI 输出区
营销策略卡片
营销图片区域
Prompt Editor
Responsive Layout
```

桌面端采用：

```text
产品输入
约 32%

AI 工作区
约 68%
```

AI 工作区内部进一步分成：

```text
营销策略 40%
+
图片 / Prompt 60%
```

---

### `frontend/app.js`

负责前端交互以及与 FastAPI 通信。

主要功能：

```text
图片上传与预览
↓
收集表单数据
↓
调用 /generate
↓
展示 AI 结果
↓
保存 Detection Label
↓
修改 Image Prompt
↓
调用 /regenerate-image
↓
更新营销图片
↓
下载最终图片
```

---

### `backend/main.py`

FastAPI 后端入口。

负责接收前端请求，并连接 Workflow。

主要接口：

```text
POST /generate
```

第一次生成完整营销创意。

```text
POST /regenerate-image
```

用户修改 Prompt 后重新生成营销图片。

---

### `backend/workflow/ad_generation.py`

整个项目的核心 Workflow Orchestrator。

它负责协调：

```text
DeepSeek
Agnes
Grounding DINO
SAM
ImageCompositor
```

而不是让前端直接调用不同模型。

核心思想：

```text
Frontend
   ↓
FastAPI
   ↓
AdGenerationWorkflow
   ↓
Different AI Providers
```

这样可以降低不同模型之间的耦合度。

---

### `backend/providers/llm/deepseek.py`

DeepSeek Provider。

主要负责语言理解与营销决策：

```text
产品参数分析
目标受众分析
卖点提炼
中英文广告文案
营销方向
Image Prompt
Detection Label
```

---

### `backend/providers/vision/agnes.py`

Agnes Vision Provider。

主要负责：

```text
读取真实产品图片
↓
分析产品视觉特征
↓
返回视觉描述
```

视觉分析结果随后提供给 DeepSeek 和检测流程使用。

---

### `backend/providers/image/agnes.py`

Agnes Image Provider。

主要负责 AI 图片生成。

当前正式 Workflow 中主要生成：

```text
EMPTY PRODUCT PHOTOGRAPHY BACKGROUND
```

而不是重新生成真实商品。

这样可以降低 AI 修改商品外观的风险。

---

### `backend/providers/background/product_segmenter.py`

负责真实商品检测和分割。

主要使用：

```text
Grounding DINO
+
SAM
```

Workflow：

```text
Detection Label
      ↓
Grounding DINO
      ↓
Bounding Box
      ↓
SAM
      ↓
Product Mask
      ↓
Transparent Product PNG
```

---

### `backend/providers/background/remover.py`

早期用于商品背景移除的模块。

项目迭代过程中发现，通用背景移除对于部分复杂商品结构的控制能力有限，因此正式 Workflow 进一步采用：

```text
Grounding DINO + SAM
```

进行更明确的目标检测与商品分割。

---

### `backend/providers/compositor/image_compositor.py`

负责最终图片合成。

输入：

```text
Transparent Real Product
+
AI Generated Background
```

输出：

```text
Final Marketing Image
```

这一层使用确定性的图像处理逻辑，而不是再次让生成模型重画商品。

---

### `.env`

保存 API Key 等环境变量。

例如：

```text
DEEPSEEK_API_KEY
AGNES_API_KEY
```

`.env` 不上传 GitHub。

---

### `.gitignore`

用于避免敏感信息和本地环境文件上传 GitHub。

例如：

```text
.env
.venv/
__pycache__/
```

---

### `requirements.txt`

记录 Python 项目依赖，例如：

```text
FastAPI
Uvicorn
OpenAI
Requests
Pillow
PyTorch
Transformers
Grounding DINO / SAM related dependencies
```

---

## 9. 技术栈

| Layer | Technology |
|---|---|
| Frontend | HTML / CSS / JavaScript |
| Backend | FastAPI / Python |
| LLM | DeepSeek |
| Vision Analysis | Agnes Vision |
| Image Generation | Agnes Image |
| Object Detection | Grounding DINO |
| Segmentation | SAM |
| Image Processing | Pillow |
| API Communication | REST API / FormData |

---

## 10. 项目中的几个关键设计

### 1. Multi-model Workflow

没有让一个模型负责所有任务，而是按照能力拆分：

```text
DeepSeek
→ 理解、分析、营销策略、Prompt

Agnes Vision
→ 产品视觉理解

Grounding DINO
→ 产品定位

SAM
→ 商品分割

Agnes Image
→ 创意背景生成

ImageCompositor
→ 确定性图片合成
```

---

### 2. Structured Output

营销分析不是直接返回一大段自然语言，而是使用结构化 JSON。

例如：

```json
{
  "target_audience": "...",
  "selling_points": [],
  "ad_copy": {
    "zh": "...",
    "en": "..."
  },
  "marketing_direction": "..."
}
```

方便后端解析并映射到前端不同 UI 模块。

---

### 3. Provider Layer

不同模型被封装成独立 Provider。

例如：

```text
providers/llm/
providers/vision/
providers/image/
providers/background/
providers/compositor/
```

这样未来更换模型时，不需要重写整个 Workflow。

例如理论上可以：

```text
Agnes Image
      ↓
Other Image Provider
```

而保持前端和核心 Workflow 结构基本不变。

---

### 4. Human-in-the-loop

AI 负责生成第一版创意，但最终 Prompt 可以由用户修改。

```text
AI Generate
    ↓
Human Review
    ↓
Edit Prompt
    ↓
Regenerate
```

相比完全自动化，这种设计更适合营销创意场景。

---

### 5. Product Fidelity

项目没有完全依赖 Generative AI 重画商品。

而是：

```text
Real Product
+
AI Background
```

通过视觉分割和图片合成尽量保留真实商品本身。

---

## 11. 当前 MVP

目前已经完成：

- [x] 产品信息输入
- [x] 产品图片上传
- [x] 产品视觉分析
- [x] AI 目标受众分析
- [x] AI 卖点提炼
- [x] 中英文广告文案生成
- [x] Image Prompt 生成
- [x] Grounding DINO 商品检测
- [x] SAM 商品分割
- [x] Agnes AI 背景生成
- [x] 真实商品与 AI 背景合成
- [x] Prompt 人工编辑
- [x] 图片重新生成
- [x] 营销图片下载
- [x] Web Demo


## 12. 项目价值

这个项目主要验证的不是“能否调用一个 AI API”，而是：

> 如何把 LLM、Vision Model、Object Detection、Segmentation、Image Generation 和传统程序逻辑组合成一个完整、可交互、可迭代的 AI 产品 Workflow。

在这个过程中，我重点实践了：

- AI Workflow Design
- Prompt Engineering
- Structured Output
- Multi-model Orchestration
- Provider Architecture
- Computer Vision Pipeline
- Human-in-the-loop
- Frontend / Backend Integration
- API Integration
- MVP Iteration

---
图片展示：<img width="1630" height="965" alt="网页" src="https://github.com/user-attachments/assets/0c6690d7-36b9-49c2-82a1-6da9e3741198" />

视频演示：
https://github.com/user-attachments/assets/cb5cefee-53fe-44ba-ada2-af624a69be40




