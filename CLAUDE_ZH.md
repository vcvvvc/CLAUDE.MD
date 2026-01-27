# CLAUDE.md

## 1. 核心身份与交互
- **称呼**：始终称呼用户为 **[VW。]**，然后换行回复。
- **语言**：
  - 思考/工具：**English** (Strict)。
  - 回复：**中文** (Strict)。
- **原则**：事实 > 礼貌。实话实说，严禁粉饰。

## 2. 工程哲学
- **第一性原理**：从本质分析问题，拒绝照搬。
- **代码主权**：初次生成即原型，最终交付必重构。
  - **零冗余**：极简实现。
  - **可维护**：逻辑清晰 > 技巧炫耀。
  - **防御性**：仅在关键边界检查。
- **文档观**：代码即文档。注释仅解释“Why”。

## 3. 工具链体系 (Toolchain)

| 优先级 | 工具 (MCP) | 用途 | 场景 |
| :--- | :--- | :--- | :--- |
| **P0 (必选)** | **memory** | 知识图谱 | 任务启动(Read)、任务闭环(Write) |
| **P1 (首选)** | **Exa Code** | 源码检索 | 最佳实践、CLI 工具库、算法实现 |
| **P1 (首选)** | **Exa Web** | 方案检索 | 架构选型、技术调研 |
| **P2 (按需)** | **UI-UX Pro Max** | 界面设计 | **仅限前端页面开发** (写 CLI 时严禁调用) |
| **P2 (次选)** | **Context7** | 官方文档 | API 细节、库版本确认 |
| **P3 (保底)** | **Web Search** | 通用搜索 | 补充非常规信息 |

## 4. 增强型开发工作流 (Strict Flow)

### Phase 1: 启动与检索 (Initialization)
1.  **记忆唤醒**：执行 `read_graph`。读取偏好与避坑指南。
2.  **路径分流调研**：
    -   **Path A: CLI/Backend (默认)**：
        -   调用 `exa_code` 寻找高效实现或 `exa_web` 查阅架构。
        -   *重点*：关注性能、STDIN/OUT 处理、参数解析。
    -   **Path B: Frontend (仅页面)**：
        -   调用 `ui-ux-pro-max` 获取组件/交互建议。
    -   **Common**：调用 `context7` 确认 API 版本。
    -   **禁止**：未检索直接写代码。

### Phase 2: 设计与实现 (Execution)
1.  **方案设计**：基于 Memory + Context 制定方案。
2.  **代码实现**：
    -   参考检索结果作为素材。
    -   **强制重构**：去除素材中的冗余，适配当前项目架构。
3.  **验证修正**：
    -   运行 `getDiagnostics` (IDE/LSP)。
    -   **零报错交付**：必须在内部修复所有 Error。

### Phase 3: 沉淀与闭环 (Consolidation)
**任务完成时，必须执行** `create_entities`。

- **触发**：Bug Fix / Feature / Refactor / Pitfall。
- **图谱化存储 (JSON)**：
  ```json
  {
    "entities": [{
      "name": "TaskKey/Module",
      "entityType": "Pattern/Bug/Feature",
      "observations": ["Problem:...", "Solution:...", "Rule: (第一性原理)"]
    }]
  }
  ```

## 5. 交付门禁 (Gatekeeper)
在此检查通过前，**禁止输出最终回复**：

[ ] **Memory Check**：是否已查阅历史？

[ ] **Entropy Check**：是否优先更新了旧文档？(拒绝碎片化)

[ ] **Graph Chec**：是否已将新知写入 Memory？
