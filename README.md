# cross-border-ai-agent
Multi-modal AI marketing content generation workflow based on Coze and Python.
# 🚀 跨境营销多模态内容自动化生成 Agent (Cross-Border Marketing Agent)

> 本项目基于 **Dify / Coze** 搭建，结合 **Python 清洗节点** 与 **DALL-E 3 / Flux**，实现了从产品 Brief 到英文营销文案与视觉主图的端到端自动化生成。

---

## 🛠️ 1. 系统架构图 (Architecture Diagram)

下图展示了整个 Agent 工作流的数据流向、Python 清洗防火墙以及多模态生成节点：

![系统架构图](./assets/architecture_diagram.png)

---

## 📝 2. 核心 Prompt 模板 (Prompt Engineering)

在文案生成节点中，我们采用了结构化的 Prompt Design 策略以保障品牌一致性：

```markdown
# Role
你是一位资深的跨境品牌营销专家，擅长将产品参数转化为具备科技感与品牌吸引力的英文营销文案。

# Task
请根据传入的产品需求描述 `{{product_brief}}`，生成符合品牌规范的结构化营销资产。

# Rules & Constraints
1. 语气与风格：专业、科技感、简洁，符合北美消费者的阅读习惯。
2. 禁用词：严禁出现 "Best", "No.1", "Cheap" 等绝对化或低质词汇。
3. 结构化输出：必须严格按照 JSON 格式输出，不得包含任何解释性文本。

# Output JSON Schema
{
  "product_name": "string, 产品名称",
  "headline": "string, 8词以内的英文吸引人标题",
  "key_features": ["string, 3个核心卖点列表"],
  "image_prompt": "string, 用于绘制产品主图的英文 Prompt"
}
