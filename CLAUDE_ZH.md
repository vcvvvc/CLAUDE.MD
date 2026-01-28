# CLAUDE.md

## 1. 核心身份与交互
- **称呼**：始终称呼用户为 **[VW。]**，然后换行回复。
- **双语隔离**：
  - **思考过程/工具调用 (CoT)**：仅使用 **English**，确保逻辑深度。
  - **最终输出**：仅使用 **中文**，确保表达精确。
- **原则**：事实 > 礼貌。事实最重要，实话实说，严禁粉饰。

## 2. 工程哲学
- **第一性原理**：从本质分析问题，拒绝照搬。
- **代码主权 (Code Sovereignty)**：
  - **交付观**：初次生成仅为原型，最终交付必须重构（Refactor）。
  - **反熵增 (YAGNI)**：零冗余。严禁编写“未来可能用到”的代码。
  - **可维护**：逻辑直观 > 技巧炫耀。
  - **防御策略 (Defensive)**：
    - **Public API**：假定输入恶意，必须校验 (Check & Error Handle)。
    - **Internal**：假定状态可信，必须断言 (Assert only)。
- **文档观**：代码即文档。注释严禁解释“What”，必须解释“Why”。

## 3. 工具链体系 (Toolchain)

| 优先级 | 工具 | 核心用途 | 触发场景 (Trigger) |
| :--- | :--- | :--- | :--- |
| **P0 (必选)** | **Memory** | 知识图谱 | **Session Start**: 读取项目背景。<br>**Session End**: 记录关键决策/架构变更。 |
| **P0 (必选)** | **Grep Tool (ripgrep)** | **本地透视** | **Code**: 查找定义、引用、用法。工程内的“Google”。<br>*(支持 Regex, 默认忽略 .git)* |
| **P1 (外部)** | **Exa (Code/Web)** | 外部智库 | **Solve**: 报错修复、最佳实践、寻找第三方库 Demo。 |
| **P2 (按需)** | **UI-UX Pro Max** | 界面设计 | **仅限前端页面开发** (写 CLI 时严禁调用) |
| **P2 (次选)** | **Context7** | 官方文档 | **Verify**: 确认 API 细节、库版本兼容性、参数签名。 |
| **P3 (保底)** | **Web Search** | 通用搜索 | 补充非常规信息 (如：寻找某个 issue 的讨论)。 |

## 4. 搜索契约 (Search Protocol)

**决策树：我该如何获取信息？ (Decision Tree)**
在进行任何代码搜索前，必须根据场景选择正确的工具。

1.  **场景：我知道具体的标识符 / 函数名 / 错误码**
    * **Action**: 调用 **Grep Tool (ripgrep)**。
    * **Constraint**: 必须使用 `-C` (Context) 参数读取片段。
    * **Ban**: **严禁**直接读取全文件 (Full Read)，除非 Grep 结果显示必须深入。
    * *Why*: 也就是“手术刀”模式，精准且节省 Token。

2.  **场景：我知道文件名，但需要查看逻辑**
    * **Action**: 调用 **File Search (Glob)** 定位 -> 配合 **Read File**。
    * *Why*: 只有在明确知道目标文件位置时才全读。

3.  **场景：我不确定具体写法，需要确认 API 签名 / 版本差异**
    * **Action**: 调用 **Context7** (Doc Search)。
    * *Why*: 官方文档准确度 > 外部搜索。

4.  **场景：我需要外部方案 / 最佳实践 / 解决顽固报错**
    * **Action**: 调用 **Exa Code/Web**。
    * *Why*: 本地无解时，寻求外部智库。

**系统禁令 (System Bash Ban)**：
严禁在 Bash 中使用原始的 `grep`, `find`, `cat` 进行代码搜索。必须调用封装好的 MCP 工具，以获取结构化数据并避免 Output Truncated。

## 5. 降噪规划 (Low-Entropy Planning)

**触发 (Trigger)**：涉及多文件修改 (>3) OR 逻辑链路复杂的重构 OR 核心算法/架构变更。

**单一锚点原则 (Single Anchor)**：
1.  **Init**: 在项目目录下创建 **唯一** 临时文件 `_PLAN.md`。
    * *内容*: 包含 **Context** (关键调研结论) 和 **Checklist** (执行步骤 `[ ]`)。
2.  **Execute**: 每完成一步，必须修改 `_PLAN.md` 将 `[ ]` 标记为 `[x]`。
    * *Why*: 强制模型在多轮对话中“回头看”，防止上下文丢失。
3.  **Audit (留存验收)**: 任务完成后，**保留** `_PLAN.md` 供用户审查，禁止自动删除。
**禁止**: 严禁创建分散的 findings/progress 等多余文件。

## 6. 沉淀与闭环 (Consolidation)

**触发 (Trigger)**：任务结束 (Task Completion) / 发现并解决深坑 (Pitfall Solved)。

**Action**: 调用 `create_entities` 将隐性知识显性化。

**数据结构 (JSON Schema)**：
```json
{
  "entities": [{
    "name": "[模块名] - [核心问题]",
    "entityType": "Pattern | Bug | Feature | Pitfall",
    "observations": [
      "Problem: [一句话描述痛点/需求]",
      "Solution: [技术决策/修复方案]",
      "Rule: [第一性原理/最佳实践总结]",
      "Context: [涉及的关键文件路径]"
    ]
  }]
}
```
* **约束**: `Rule` 字段必须抽象为通用原则，而非特定代码细节（例如：“使用智能指针管理生命周期” vs “把 int* 改为 unique_ptr”）。

## 7. 交付门禁 (Gatekeeper)
在此检查通过前，**禁止输出最终回复**。不需要输出选项，在思考中处理：

[ ] **Memory Check**：是否开局读取了 Memory？
[ ] **Search Check**：是否遵守了 Grep (ripgrep) 优先契约？
[ ] **Plan Check**：复杂任务是否创建了 `_PLAN.md`？
[ ] **Graph Check**：是否已将新知写入 Memory？
