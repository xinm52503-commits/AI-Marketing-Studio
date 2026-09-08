# cross-border-ai-agent
Multi-modal AI marketing content generation workflow based on Coze and Python.
# 🚀 跨境营销多模态内容自动化生成 Agent (Cross-Border Marketing Agent)

> 本项目基于 **Dify / Coze** 搭建，结合 **Python 清洗节点** 与 **DALL-E 3 / Flux**，实现了从产品 Brief 到英文营销文案与视觉主图的端到端自动化生成。

---

## 🛠️ 1. 系统架构图 (Architecture Diagram)

下图展示了整个 Agent 工作流的数据流向、Python 清洗防火墙以及多模态生成节点：
```mermaid
flowchart TD
    A[用户输入 Product Brief] --> B[Dify / Coze 工作流]
    B --> C[Python 清洗防火墙]
    C --> D[营销文案与多模态生成]

![系统架构图]！<img width="1422" height="276" alt="image" src="https://github.com/user-attachments/assets/2858d7be-6ec5-4bea-99cf-a4f7b409f15e" />

## 📝 2. 核心 Prompt 模板 (Prompt Engineering)

在文案生成节点中，我们采用了结构化的 Prompt Design 策略以保障品牌一致性：

```markdown
# Role
你是一名资深的 [填写角色，如：跨境电商营销专家 / Python 高级架构师]。

# Context & Goal
- **业务背景**：[说明上下文，如：我们需要为安克出海产品批量生成符合北美本地化习惯的 Amazon 营销资产]
- **核心目标**：[说明明确目标，如：根据传入的产品需求说明，提取核心卖点并输出结构化的营销文案与生图 Prompt]

# Input Data
系统将接收以下格式的输入数据：
- **产品描述 (Product Brief)**: `{{product_brief}}`
- **目标受众 (Target Audience)**: `{{target_audience}}`

# Workflow Steps
请严格按照以下步骤依次执行任务：
1. **分析与提取**：剖析 `product_brief` 中的核心功能与用户痛点，挑选出 3 个最具吸引力的卖点。
2. **文案创作**：撰写句式极简、语气地道的英文 Headline（控制在 8 词以内）。
3. **视觉转换**：将产品视觉特征转化为适用于 DALL-E 3 / Flux 的英文 Prompt。
4. **格式封装**：将生成结果封装为符合 Schema 要求的 JSON 数据，不附加任何额外文本。

# Constraints & Rules
- **语气风格 (Tone & Manner)**：专业、科技感、简洁，严禁使用夸大宣传。
- **禁用词列表 (Negative Words)**：严禁出现 "Best"、"No.1"、"Top-tier"、"Cheap" 等绝对化或低质词汇。
- **技术约束**：必须输出合法 JSON 格式，严格包含指定字段，不得在外层输出任何解释性 Markdown 闲聊。

# Output Schema
输出内容必须严格遵循以下 JSON 结构：
```json
{
  "product_name": "string, 产品标准英文名称",
  "headline": "string, 8词以内的英文吸引人标题",
  "key_features": [
    "string, 卖点1",
    "string, 卖点2",
    "string, 卖点3"
  ],
  "image_prompt": "string, 用于生图模型的英文 Prompt，需包含构图、光影及风格"
}
当然可以。下面我直接整理成一份**适合 GitHub README / Product Case Study 使用的 Markdown 文档**。我会把前面的内容重新组织成比较专业的产品项目结构，而不是简单复制，方便你之后继续往里面添加技术架构、代码、Demo 和迭代记录。

你可以直接保存为：

```text
README.md
```

````markdown
# AI Marketing Creative Agent

> An AI-powered marketing creative platform that transforms product information into advertising concepts, visual assets, and short-form video through a multi-model AI workflow.

---

## 1. Project Overview

AI Marketing Creative Agent is an AI-powered advertising content generation platform designed for small brands, cross-border e-commerce sellers, social media marketers, and marketing agencies.

The platform aims to solve a common problem:

> Users know they need more advertising content, but creating high-quality advertising creatives is expensive, time-consuming, and requires specialised skills.

Instead of providing only an AI video generation tool, this project focuses on building an end-to-end **AI Marketing Creative Workflow**:

```text
Product Information
        ↓
AI Marketing Strategy
        ↓
Creative Concepts
        ↓
Structured JSON
        ↓
Image Generation
        ↓
Reference Image
        ↓
Video Prompt Optimisation
        ↓
Video Generation
        ↓
Advertising Creative
        ↓
Publish
        ↓
Performance Analysis
        ↓
Creative Iteration
````

The long-term goal is to evolve from an **AI content generation tool** into an **AI Creative Optimization Agent**.

---

# 2. Project Motivation

## 2.1 Increasing Demand for Marketing Content

Modern marketing channels such as:

* TikTok
* Instagram Reels
* YouTube Shorts
* Amazon
* TikTok Shop
* Shopify
* Independent websites

increasingly rely on short-form video and visual content.

However, brands do not simply need one video.

A typical marketing process looks more like:

```text
1 Product
   ↓
Multiple Creative Concepts
   ↓
Multiple Advertising Variations
   ↓
Different Platforms
   ↓
Continuous Testing
   ↓
Performance Optimisation
```

This creates a large amount of creative production work.

---

## 2.2 Existing AI Tools Are Fragmented

Users currently need to combine multiple tools:

```text
ChatGPT
    ↓
Creative Idea

Image Generation Tool
    ↓
Product Visual

Video Generation Tool
    ↓
Advertisement Video

Canva / CapCut
    ↓
Editing

TikTok / Instagram
    ↓
Publishing
```

The problem is no longer simply a lack of AI capabilities.

The problem is:

> **AI tools are powerful but fragmented. Users still need to manually coordinate the entire workflow.**

---

## 2.3 From "Generation" to "Decision + Generation"

Most AI creative tools focus on:

> "Generate a video."

However, the real marketing problem is:

> "What kind of video should I create?"

Therefore, the product focuses on both:

* Creative decision-making
* Content generation
* Creative iteration
* Performance optimisation

The long-term product loop is:

```text
Think
  ↓
Create
  ↓
Test
  ↓
Learn
  ↓
Improve
  ↓
Create Again
```

---

# 3. Core User Problems

| User Pain Point               | User Question                                     | Product Solution            |
| ----------------------------- | ------------------------------------------------- | --------------------------- |
| Lack of creative ideas        | "What kind of advertisement should I create?"     | AI Creative Strategy        |
| Lack of prompt-writing skills | "How should I describe the visual?"               | Prompt Generation Agent     |
| Fragmented AI tools           | "Why do I need to switch between multiple tools?" | Multi-model Workflow        |
| Slow production               | "Why does one advertisement take hours?"          | End-to-end AI Generation    |
| Lack of optimisation          | "How do I know whether the advertisement works?"  | Analytics + AI Optimisation |

### Core Value Proposition

The product is not simply designed to save users a few copy-and-paste operations.

Its core value is:

> **Reduce the time, cost, and expertise required to transform a product into a testable advertising creative.**

---

# 4. Target Users

## 4.1 Cross-border E-commerce Sellers

Examples:

* Amazon sellers
* TikTok Shop sellers
* Shopify sellers
* DTC brands

### Characteristics

* Large number of products
* High demand for marketing content
* Limited creative teams
* Limited marketing budgets
* Strong focus on ROI

### Core Need

> Product information → Advertising Creative

---

## 4.2 Small Brands / DTC Brands

Typical team size:

```text
3–10 people
```

Possible team structure:

```text
Founder
Marketing
Sales
Part-time Designer
```

These teams may not have a dedicated creative department.

### Core Need

> Generate high-quality advertising creatives quickly without building a large creative team.

---

## 4.3 Social Media Managers

These users may already understand design and marketing.

Their biggest problem is often:

> **Creative production capacity.**

For example:

```text
Weekly Content Requirements

TikTok       5 videos
Instagram    3 videos
Facebook     3 videos
```

The AI Agent becomes a creative production assistant.

---

## 4.4 Marketing Agencies

Agencies can become an important high-value customer segment.

A typical agency may manage:

```text
10 Clients
×
Multiple Products
×
Multiple Creative Variations
```

They need:

* Batch generation
* Multiple workspaces
* Team collaboration
* Brand management
* Creative templates
* API access

### Potential Positioning

> AI Creative Infrastructure for Marketing Agencies

---

# 5. User Personas

| Persona              | Main Goal                    | Main Pain Point            | Key Feature          |
| -------------------- | ---------------------------- | -------------------------- | -------------------- |
| E-commerce Seller    | Sell products                | Lack of creative resources | AI Ad Generator      |
| Small Brand          | Build brand awareness        | Small marketing team       | AI Creative Director |
| Social Media Manager | Produce content consistently | Low creative capacity      | Creative Automation  |
| Marketing Agency     | Manage multiple clients      | High production workload   | Batch + Team + API   |

---

# 6. User Scenarios → Product Features

A key product design principle is:

> **User Scenario → Pain Point → Feature**

---

## Scenario 1: User Has a New Product

### User Action

Upload:

* Product image
* Product description
* Target market
* Key selling points

### AI Workflow

```text
Product Information
        ↓
Product Analysis
        ↓
Target Audience
        ↓
Selling Points
        ↓
Advertising Strategy
        ↓
Creative Concepts
```

### Product Feature

**Creative Strategy Agent**

---

# 7. Scenario 2: User Selects a Creative Concept

The system generates multiple creative directions.

Example:

```text
Creative Concept A
Problem → Solution

Creative Concept B
Before → After

Creative Concept C
Lifestyle Story

Creative Concept D
Product Demonstration
```

The user selects one.

The AI then generates:

```text
Hook
↓
Story
↓
Visual Style
↓
Image Prompt
↓
Video Prompt
```

### Product Feature

**AI Creative Director**

---

# 8. Scenario 3: Generate Visual Reference

The system converts the creative concept into a structured JSON object.

Example:

```json
{
  "product_name": "Luxury Skincare Serum",
  "creative_concept": "Premium Morning Skincare Ritual",
  "image_prompt": "A luxury skincare serum bottle placed on white marble...",
  "video_prompt": "Slow cinematic camera movement around the serum bottle...",
  "camera": {
    "movement": "slow dolly in",
    "angle": "low angle"
  },
  "lighting": "soft morning sunlight",
  "style": "luxury commercial",
  "duration": 8
}
```

The `image_prompt` is then sent to an image generation model.

```text
Image Prompt
      ↓
Image Generation Model
      ↓
Reference Image
```

### Product Feature

**AI Visual Studio**

---

# 9. Scenario 4: Generate Advertising Video

The system combines:

* Original creative concept
* Image prompt
* Reference image
* Video prompt

and sends them to a Video Director Agent.

The Video Director Agent generates a final structured video prompt.

Example:

```json
{
  "final_video_prompt": "...",
  "camera_motion": "slow cinematic dolly in",
  "lighting": "soft morning sunlight",
  "duration": 8,
  "aspect_ratio": "9:16"
}
```

The final inputs are:

```text
Reference Image
+
Final Video Prompt
+
Video Parameters
        ↓
Video Generation Model
        ↓
Final Advertisement
```

### Product Feature

**AI Video Generator**

---

# 10. Scenario 5: User Is Not Satisfied with the Result

Instead of asking users to rewrite prompts manually, provide one-click creative iteration.

Possible options:

```text
More Premium
More Energetic
More Emotional
More Realistic
More Gen-Z
More Luxury
More Minimal
More UGC
```

The AI automatically modifies the creative direction.

### Product Feature

**Creative Iteration**

---

# 11. End-to-End Product Workflow

```text
                    Product Information
                           │
                           ▼
                  ┌─────────────────┐
                  │ AI Strategy     │
                  │ Agent           │
                  └────────┬────────┘
                           │
                           ▼
                    Creative Ideas
                           │
                           ▼
                  ┌─────────────────┐
                  │ AI Creative     │
                  │ Director        │
                  └────────┬────────┘
                           │
                           ▼
                   Creative Concept
                           │
                ┌──────────┴──────────┐
                ▼                     ▼
        Image Generation       Video Prompt
                │                     │
                ▼                     │
        Reference Image               │
                │                     │
                └──────────┬──────────┘
                           ▼
                  Video Generation
                           │
                           ▼
                    Advertisement
                           │
                           ▼
                        Publish
                           │
                           ▼
                     Performance
                           │
                           ▼
                  AI Optimisation
                           │
                           ▼
                   New Creatives
```

---

# 12. Multi-Model Architecture

The system uses multiple specialised AI models rather than relying on a single model.

```text
                    User Input
                        │
                        ▼
               ┌────────────────┐
               │ LLM #1         │
               │ Creative Agent │
               └───────┬────────┘
                       │
                       ▼
                Structured JSON
                       │
                       ▼
               ┌────────────────┐
               │ Image Model    │
               └───────┬────────┘
                       │
                       ▼
                 Reference Image
                       │
                       ▼
               ┌────────────────┐
               │ LLM #2         │
               │ Video Director │
               └───────┬────────┘
                       │
                       ▼
                 Video Prompt
                       │
                       ▼
               ┌────────────────┐
               │ Video Model    │
               └───────┬────────┘
                       │
                       ▼
                 Final Video
```

The system can be implemented using a Python backend and API-based model orchestration.

---

# 13. MVP

The first version should remain simple.

## V0 — Prototype

```text
Product Input
      ↓
LLM
      ↓
Structured JSON
      ↓
Image Generation
      ↓
Reference Image
      ↓
Video Generation
      ↓
Final Video
```

### Goal

> Validate whether the end-to-end workflow works.

---

# 14. V1 — Creative Agent

Add multiple creative concepts.

```text
Product
   ↓
AI
   ↓
3–5 Creative Concepts
   ↓
User Selects One
   ↓
Generate Advertisement
```

### Goal

> Validate whether users like AI-generated creative strategies.

---

# 15. V2 — Creative Library

Add:

* Creative History
* Templates
* Remix
* Brand Kit
* Saved Prompts
* Project Management

### Goal

> Increase user retention and content reuse.

---

# 16. V3 — Creative Optimization Agent

Connect advertising performance data.

```text
Advertisement
      ↓
Publish
      ↓
Performance Data
      ↓
AI Analysis
      ↓
Identify What Works
      ↓
Generate New Creative
      ↓
Test Again
```

The product evolves from:

> AI Content Generator

to:

> **AI Creative Optimization Agent**

---

# 17. Community Model

The community is based on **Creative Templates + Remix**.

Users can publish:

* Creative Concepts
* Advertising Templates
* Prompts
* Generated Videos
* Hooks
* Visual Styles

Example:

```text
Luxury Product Unboxing
```

Another user can click:

```text
Remix This Creative
```

The system automatically:

```text
Original Creative
      ↓
Copy Template
      ↓
Replace Product
      ↓
Adapt Prompt
      ↓
Generate New Advertisement
```

---

# 18. Creative Marketplace

In the future, high-performing creative templates can become marketplace assets.

Example:

```text
UGC Product Review Template
$2.99

Luxury Product Advertisement
$4.99

TikTok Problem-Solution Template
Free
```

Creators can receive a percentage of revenue.

Potential revenue split:

```text
Creator     70%
Platform    30%
```

This creates an additional business model:

> **SaaS + Creative Marketplace**

---

# 19. Community Growth Loop

The community can create a self-reinforcing growth loop:

```text
Users
  ↓
Create Ads
  ↓
Share Creatives
  ↓
Community Templates
  ↓
Social Media Content
  ↓
New Users
  ↓
Create More Ads
  ↓
More Community Content
```

This can gradually reduce dependence on paid acquisition.

---

# 20. Business Model

## 20.1 Freemium

### Free

Possible limitations:

* Limited creative generations
* Limited image generations
* One free video
* Watermark
* Community templates

Goal:

> Let users experience the core product value before paying.

---

## 20.2 Pro

Potential pricing:

```text
$19–39 / month
```

Features:

* More video credits
* HD generation
* No watermark
* Brand Kit
* Creative history
* More AI models
* Commercial usage

---

## 20.3 Business

Potential pricing:

```text
$99+ / month
```

Features:

* Team workspace
* Multiple brands
* Batch generation
* Brand management
* Analytics
* Collaboration
* Shared creative library

---

## 20.4 Agency

Custom pricing.

Features:

* Multiple clients
* Multiple workspaces
* Batch generation
* API access
* Team permissions
* White-label options
* Advanced analytics

---

# 21. Unit Economics

The main variable costs are expected to come from:

```text
LLM API
+
Image Generation API
+
Video Generation API
```

Video generation is likely to be the most significant variable cost.

Therefore, the pricing model should not rely only on unlimited generations.

A credit-based system is more sustainable:

```text
Subscription
      +
Generation Credits
      +
Optional Credit Top-up
```

---

# 22. Validation Strategy

The product should not be fully developed before validating demand.

The first goal is to answer:

> **Will users actually use AI to generate advertising creatives?**

---

## Validation 1 — Landing Page

Create a simple landing page:

```text
Turn Your Product
Into High-Quality Ads

Upload your product.
AI creates your advertising creative.

[ Generate My Ad ]
```

Measure:

```text
Visitors
   ↓
Click "Generate"
   ↓
Upload Product
   ↓
Generate Advertisement
   ↓
Return to Product
```

---

# 23. Validation 2 — Concierge MVP

The first version does not need to be fully automated.

Users upload a product.

The backend manually triggers the AI workflow.

Then the generated advertisement is delivered to the user.

This allows validation of:

* Output quality
* User satisfaction
* Willingness to use again
* Willingness to pay

before investing heavily in infrastructure.

---

# 24. Key Product Metrics

## Activation

```text
Sign Up
  ↓
Upload Product
  ↓
Generate First Advertisement
```

---

## Time to Value

Measure:

> How long does it take from product upload to the first usable advertisement?

Target:

```text
< 5 minutes
```

---

## Generation → Publish Rate

Measure:

> What percentage of generated creatives are actually published?

This is more meaningful than simply measuring generation volume.

---

## Retention

Measure whether users return:

```text
Day 1
Day 7
Day 30
```

A strong signal is:

> Users return every week to generate new advertising creatives.

---

## Conversion

Important funnel:

```text
Website Visit
      ↓
Sign Up
      ↓
Upload Product
      ↓
Generate Creative
      ↓
Publish Creative
      ↓
Subscribe
```

---

# 25. Product Iteration Framework

Product decisions should follow:

```text
Hypothesis
    ↓
Build
    ↓
Measure
    ↓
Learn
    ↓
Iterate
```

Example:

### Hypothesis

> Users want multiple creative concepts before generating a video.

### MVP

Generate 3 concepts.

### Metric

Measure:

```text
Concept Selection Rate
Video Generation Rate
User Satisfaction
```

### Decision

If users consistently select one concept type:

> Improve that creative category.

---

# 26. Go-To-Market Strategy

The initial target market should be narrow.

Instead of:

> AI advertising for everyone

start with:

> **AI Ads for Cross-border E-commerce Sellers**

Potential initial users:

* Amazon sellers
* TikTok Shop sellers
* Shopify brands
* Small DTC brands

This market has a clear connection:

```text
Product
 ↓
Advertisement
 ↓
Traffic
 ↓
Conversion
 ↓
Revenue
```

---

# 27. Cold Start Strategy

The initial GTM strategy should focus on **content-led growth** rather than expensive advertising.

Potential channels:

* TikTok
* Instagram
* YouTube Shorts
* LinkedIn
* Product Hunt
* Reddit communities
* E-commerce communities

---

# 28. Social Media Content Strategy

The product itself should become a content-generation engine.

---

## Content Type 1 — Before / After

```text
Original Product Image
        ↓
AI
        ↓
Professional Advertisement
```

This demonstrates product value immediately.

---

## Content Type 2 — Prompt → Result

Example:

```text
I gave AI this product...

This is what it created.
```

Then show:

```text
Product
↓
Prompt
↓
Generated Image
↓
Generated Video
```

---

## Content Type 3 — AI Challenge

Example:

> "I gave AI 5 random Amazon products and asked it to create TikTok ads."

Then show:

```text
Product 1 → Advertisement
Product 2 → Advertisement
Product 3 → Advertisement
Product 4 → Advertisement
Product 5 → Advertisement
```

This simultaneously acts as:

* Product Demo
* Social Content
* User Education
* Marketing

---

# 29. Product-Led Growth Loop

The ideal growth loop is:

```text
User
 ↓
Generates Advertisement
 ↓
Publishes Advertisement
 ↓
"Made with AI Marketing Creative Agent"
 ↓
Audience Sees It
 ↓
Audience Clicks
 ↓
New User
 ↓
Generates Their Own Advertisement
```

The product output becomes the marketing channel.

---

# 30. Long-Term Competitive Advantage

The competitive advantage should not depend on a single image or video model.

Models will continue to improve and become commoditised.

Long-term differentiation should come from:

### 1. Creative Data

```text
Product
+
Creative
+
Prompt
+
Performance
```

---

### 2. Creative Templates

A library of proven advertising structures.

---

### 3. Brand Memory

The system remembers:

* Brand identity
* Product positioning
* Visual style
* Tone of voice
* Target audience
* Brand assets

---

### 4. Performance Feedback

The system learns:

```text
Creative
 ↓
Performance
 ↓
Insight
 ↓
New Creative
```

---

### 5. Community

Users contribute:

* Templates
* Creative concepts
* Prompts
* Successful advertising formats

This creates a potential network effect.

---

# 31. Product Flywheel

The long-term product flywheel is:

```text
                    Product Input
                         │
                         ▼
                AI Marketing Strategy
                         │
                         ▼
                  Creative Concepts
                         │
                         ▼
                 Image + Video AI
                         │
                         ▼
                    Advertisement
                         │
                         ▼
                       Publish
                         │
                         ▼
                    Performance
                         │
                         ▼
                 AI Optimisation
                         │
                         ▼
                  Better Creatives
                         │
                         ▼
                Community Templates
                         │
                         ▼
                     New Users
                         │
                         └───────────────┐
                                         │
                                         ▼
                                More Creative Data
```

---

# 32. Product Vision

The ultimate vision is not to build another AI video generator.

The vision is:

> **Build an AI Creative Operating System for modern marketing teams.**

The evolution can be represented as:

```text
AI Prompt Generator
        ↓
AI Content Generator
        ↓
AI Creative Agent
        ↓
AI Creative Optimization Agent
        ↓
AI Marketing Operating System
```

---

# 33. One-Sentence Product Positioning

### Short Version

> **Turn your product into high-performing advertising creatives with AI.**

### More Product-Oriented Version

> **An AI Marketing Creative Agent that transforms product information into advertising strategies, visual assets, and short-form videos through an automated multi-model workflow.**

### Long-Term Vision

> **From product information to creative strategy, content generation, performance analysis, and continuous optimisation — all in one AI-powered marketing workflow.**

---

# 34. Roadmap

| Stage | Core Capability         | Goal                             |
| ----- | ----------------------- | -------------------------------- |
| V0    | Product → Image → Video | Validate technical feasibility   |
| V1    | Creative Strategy Agent | Validate creative value          |
| V2    | Templates + Remix       | Improve retention                |
| V3    | Community               | Build network effects            |
| V4    | Performance Analytics   | Connect creation with results    |
| V5    | AI Optimisation         | Close the creative feedback loop |
| V6    | Marketplace + API       | Expand monetisation              |
| V7    | Marketing OS            | Build long-term platform         |

---

# 35. MVP Success Criteria

The MVP should answer five questions:

1. **Can AI generate usable advertising creatives?**
2. **Do users believe the output saves them time?**
3. **Will users generate more than one creative?**
4. **Will users publish the generated content?**
5. **Are users willing to pay for continued generation?**

If the answer to these questions is positive, the product has a strong foundation for further development.

---

# 36. Final Product Concept

The core product loop can ultimately be summarised as:

```text
                    THINK
                      ↓
                  CREATE
                      ↓
                    TEST
                      ↓
                   ANALYSE
                      ↓
                  OPTIMISE
                      ↓
                   CREATE
                      ↺
```

The goal is to transform advertising production from:

> **Manual + Fragmented + Expensive**

into:

> **AI-powered + Automated + Iterative + Data-driven**

---

## Conclusion

AI Marketing Creative Agent is designed around a simple principle:

> **The future of AI marketing is not simply generating more content. It is generating the right content, testing it, learning from the results, and continuously improving it.**

The initial product can start with a relatively simple multi-model workflow:

```text
LLM
 ↓
Structured JSON
 ↓
Image Model
 ↓
Reference Image
 ↓
Video Director LLM
 ↓
Video Model
 ↓
Advertisement
```

But the long-term opportunity is much larger:

```text
Strategy
   ↓
Creation
   ↓
Distribution
   ↓
Performance
   ↓
Learning
   ↓
Optimisation
   ↺
```

This creates the foundation for an AI-powered marketing creative platform rather than a simple AI content generation tool.

````

### GitHub 目录我建议你最终做成这样

```text
ai-marketing-creative-agent/
│
├── README.md
│
├── docs/
│   ├── product-strategy.md
│   ├── user-research.md
│   ├── user-personas.md
│   ├── user-journey.md
│   ├── prd.md
│   ├── business-model.md
│   ├── gtm-strategy.md
│   └── roadmap.md
│
├── src/
│   ├── llm/
│   ├── image/
│   ├── video/
│   └── workflow/
│
├── prompts/
│   ├── creative_director.md
│   └── video_director.md
│
├── examples/
│
├── .env.example
├── requirements.txt
└── README.md
````

这样以后你在 GitHub 上展示的就不只是代码，而是一套完整的 **AI 产品案例（Product Case Study）+ 技术实现 + 商业化思路**。这对于你拿它作为 AI / AI Agent 方向的求职项目会更有价值。
