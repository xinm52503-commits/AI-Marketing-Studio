# cross-border-ai-agent
Multi-modal AI marketing content generation workflow based on Coze and Python.
# 🚀 跨境营销多模态内容自动化生成 Agent (Cross-Border Marketing Agent)

> 本项目基于 **Dify / Coze** 搭建，结合 **Python 清洗节点** 与 **DALL-E 3 / Flux**，实现了从产品 Brief 到英文营销文案与视觉主图的端到端自动化生成。

---

## 🛠️ 1. 系统架构图 (Architecture Diagram)

下图展示了整个 Agent 工作流的数据流向、Python 清洗防火墙以及多模态生成节点：

![系统架构图]<img width="1521" height="587" alt="image" src="https://github.com/user-attachments/assets/67329ead-cff1-4fc8-bc02-886863c3eeb2" />


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
