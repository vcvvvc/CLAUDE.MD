### 1. Exa（URL 方式）
```bash
codex mcp add exa --url https://mcp.exa.ai/mcp
```

### 2. Context7（stdio + 参数传给 server）
```bash
codex mcp add context7 -- npx -y @upstash/context7-mcp --api-key YOUR_KEY
```

### 3. Memory（stdio + 通过 env 指定知识库文件路径）
```bash
codex mcp add memory \
  --env MEMORY_FILE_PATH=/ABS/PATH/.codex/knowledge.json \
  -- npx -y @modelcontextprotocol/server-memory
```

### 4. CCLSP（stdio + 通过 env 指定配置文件路径）
```bash
codex mcp add cclsp \
  --env CCLSP_CONFIG_PATH=/ABS/PATH/cclsp.json \
  -- npx -y cclsp@latest
```

### 5. 验证
```bash
codex mcp list
```

---

```toml
[mcp_servers.memory]
command = "npx"
args = ["-y", "@modelcontextprotocol/server-memory"]

[mcp_servers.memory.env]
MEMORY_FILE_PATH = ""

[mcp_servers.context7]
command = "npx"
args = ["-y", "@upstash/context7-mcp", "--api-key", "yourkey"]

[mcp_servers.exa]
url = "https://mcp.exa.ai/mcp"

[mcp_servers.cclsp]
command = "npx_path"
args = ["-y", "cclsp"]

[mcp_servers.cclsp.env]
# 建议用绝对路径；相对路径会受启动目录影响
# 例："/root/.codex/cclsp.json"
CCLSP_CONFIG_PATH = ".codex/cclsp.json"
```
