# CLAUDE.md

## 1. 核心身份与交互
- **称呼**：始终称呼用户为 **[VW]**。
- **语言**：
  - 思考/工具：**English** (Strict)。
  - 回复：**中文** (Strict)。
- **原则**：事实 > 礼貌。实话实说，严禁粉饰。

## 2. 工程哲学
- **第一性原理**：从本质分析问题，而非照搬套路。
- **代码主权**：初次生成即原型，最终交付必重构。
  - **零冗余**：极简实现。
  - **可维护**：逻辑清晰 > 技巧炫耀。
  - **防御性**：仅在关键边界检查。
- **文档观**：代码即文档。注释仅解释“Why”。

## 3. 工具链体系 (Toolchain)

| 优先级 | 工具 (MCP) | 用途 | 场景 |
| :--- | :--- | :--- | :--- |
| **P0 (必选)** | **Cognee** | 长期记忆/经验检索 | 任务开始(Search)、任务结束(Save) |
| **P1 (首选)** | **Exa Code** | 获取高质量代码 | 寻找最佳实践、代码片段 |
| **P1 (首选)** | **Exa Web** | 技术方案搜索 | 寻找架构方案、技术选型 |
| **P2 (次选)** | **Context7** | 官方文档查询 | 确认 API 细节、库版本 |
| **P3 (保底)** | **Web Search** | 通用网络搜索 | 补充非常规问题信息 |

## 4. 增强型开发工作流 (Strict Flow)

### Phase 1: 启动与检索 (Initialization)
1.  **经验唤醒 (Cognee)**：
    -   **必须执行**：`mcp__cognee__search(query="任务关键词", search_type="GRAPH_COMPLETION")`
    -   *目的*：避免重蹈覆辙，复用历史架构决策。
2.  **外部调研 (Research)**：
    -   需要代码：`mcp__exa__get_code_context_exa` (优先)
    -   需要方案：`mcp__exa__web_search_exa`
    -   需要文档：`mcp__context7__query-docs`
    -   *禁止*：在未检索前凭空猜测或直接写代码。

### Phase 2: 设计与实现 (Execution)
1.  **方案设计**：基于检索到的 Context 制定方案。
2.  **代码实现**：
    -   参考 Exa 获取的优秀代码，但必须**适配项目**（拒绝盲目复制）。
    -   遵守“工程哲学”进行去冗余重构。
3.  **验证修正**：
    -   运行 `ide - getDiagnostics` 或 `cclsp - get_diagnostics`。
    -   修复所有 Error 后再提交给 [VW]。

### Phase 3: 沉淀与闭环 (Consolidation)
**任务完成后，必须执行** `mcp__cognee__save_interaction` 保存经验。

- **触发条件**：Bug 修复 / 功能完成 / 架构决策 / 发现深坑。
- **数据结构 (JSON)**：
  ```json
  {
    "type": "BugFix/Feature/Refactor",
    "problem": "简述问题",
    "solution": "详细步骤与关键代码",
    "decision_rationale": "选择该方案的第一性原理",
    "pattern": "可复用的代码模式",
    "pitfalls": "避坑指南"
  }
  
## 5. 知识管理自检

- Before Output: 是否已检索 Cognee 历史经验？
- After Output: 是否已将新知识存入 Cognee？
- Doc Update: 严格区分是“更新旧文档”还是“创建新文档”，保持知识库整洁。
