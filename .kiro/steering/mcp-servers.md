# MCP Servers & Kiro Powers Reference

Dokumentasi lengkap semua MCP servers dan Kiro Powers yang terinstall.

## Overview

Project ini menggunakan:
- **22+ MCP Servers** - Tools khusus via Model Context Protocol
- **3 Kiro Powers** - Figma, Aurora PostgreSQL, Power Builder

---

## BAGIAN A: MCP SERVERS

### Kategori: Development Tools

#### 1. Desktop Commander
- **Package:** `@wonderwhy-er/desktop-commander`
- **Status:** ✅ Ready
- **Fungsi:** Terminal commands, file operations
- **Tools:** `execute_command`, `read_file`, `write_file`, `search_files`, `edit_block`

#### 2. Git MCP
- **Package:** `@modelcontextprotocol/server-git`
- **Status:** ✅ Ready
- **Fungsi:** Git version control
- **Tools:** `git_status`, `git_diff`, `git_log`, `git_commit`

#### 3. Think Tool
- **Package:** `think-tool-mcp`
- **Status:** ✅ Ready
- **Fungsi:** Enhanced AI reasoning
- **Tools:** `think`

#### 4. Playwright
- **Status:** ✅ Ready
- **Fungsi:** Browser automation & testing
- **Tools:**
  - `browser_navigate` - Navigate to URL
  - `browser_snapshot` - Capture accessibility snapshot
  - `browser_click` - Click elements
  - `browser_type` - Type text
  - `browser_fill_form` - Fill multiple form fields
  - `browser_take_screenshot` - Take screenshots
  - `browser_evaluate` - Execute JavaScript
  - `browser_file_upload` - Upload files
  - `browser_tabs` - Manage browser tabs
  - `browser_wait_for` - Wait for elements/text
  - `browser_console_messages` - Get console logs
  - `browser_network_requests` - Monitor network
- **Use Cases:** UI testing, web scraping, form automation

#### 5. Filesystem
- **Status:** ✅ Ready
- **Fungsi:** File system operations
- **Allowed Directory:** `D:\Programs\Kiro`
- **Tools:**
  - `read_text_file` / `read_media_file` - Read files
  - `read_multiple_files` - Batch read
  - `write_file` - Create/overwrite files
  - `edit_file` - Line-based edits
  - `create_directory` - Create folders
  - `list_directory` / `list_directory_with_sizes` - List contents
  - `directory_tree` - Recursive tree view
  - `move_file` - Move/rename files
  - `search_files` - Glob pattern search
  - `get_file_info` - File metadata
- **Use Cases:** File management, batch operations

#### 6. Sequential Thinking
- **Status:** ✅ Ready
- **Fungsi:** Dynamic problem-solving through structured thoughts
- **Tools:** `sequentialthinking`
- **Use Cases:** Complex multi-step reasoning, hypothesis verification

---

### Kategori: Documentation & Context

#### 7. Context7
- **Package:** `@upstash/context7-mcp@latest`
- **Status:** ✅ Ready
- **Fungsi:** Up-to-date library documentation
- **Tools:** `resolve-library-id`, `query-docs`
- **Use Cases:** Latest docs untuk FastAPI, SQLModel, Laravel, WPF .NET 8

#### 8. Telerik WPF
- **URL:** `https://mcp.telerik.com/wpf`
- **Status:** ✅ Ready (remote)
- **Fungsi:** Telerik UI for WPF documentation

---

### Kategori: Web Search & Scraping

#### 9. Tavily
- **Package:** `tavily-mcp`
- **Status:** ✅ Ready (API key configured)
- **Env:** `TAVILY_API_KEY`
- **Tools:**
  - `tavily_search` - Web search dengan filtering
  - `tavily_extract` - Extract content dari URLs
  - `tavily_crawl` - Crawl website
  - `tavily_map` - Map website structure
- **Use Cases:** Research, troubleshooting, price lookup

#### 10. Exa
- **Package:** `exa-mcp-server`
- **Status:** ✅ Ready (API key configured)
- **Env:** `EXA_API_KEY`
- **Tools:**
  - `web_search_exa` - AI-powered search
  - `get_code_context_exa` - Code-specific search
- **Use Cases:** Deep research, code context

#### 11. Firecrawl
- **Package:** `firecrawl-mcp`
- **Status:** ✅ Ready (API key configured)
- **Env:** `FIRECRAWL_API_KEY`
- **Tools:**
  - `firecrawl_scrape` - Single page scrape
  - `firecrawl_map` - Discover URLs
  - `firecrawl_crawl` - Multi-page crawl
  - `firecrawl_search` - Search + scrape
  - `firecrawl_extract` - Structured extraction
  - `firecrawl_agent` - Autonomous data gathering
- **Use Cases:** Product specs, batch scraping, structured JSON

#### 12. Fetch
- **Package:** `@modelcontextprotocol/server-fetch`
- **Status:** ✅ Ready
- **Fungsi:** Simple web content fetching

#### 13. SearXNG
- **Package:** `mcp-searxng`
- **Status:** ⚠️ Requires Docker
- **Setup:** `docker run -d -p 8080:8080 searxng/searxng`

---

### Kategori: Database

#### 14. Postgres MCP
- **Package:** `postgres-mcp` (uvx)
- **Status:** ⚠️ Requires connection string
- **Env:** `POSTGRES_CONNECTION_STRING`
- **Tools:**
  - `list_schemas` / `list_objects` / `get_object_details`
  - `execute_sql` - Read-only queries
  - `explain_query` - Query plan analysis
  - `get_top_queries` - Slow query identification
  - `analyze_workload_indexes` / `analyze_query_indexes`
  - `analyze_db_health` - Health checks

#### 15. MySQL
- **Status:** ⚠️ Requires configuration
- **Tools:**
  - `list_databases` - List databases
  - `list_tables` - List tables
  - `describe_table` - Show schema
  - `execute_query` - Read-only SQL
- **Use Cases:** MySQL database exploration

---

### Kategori: Version Control & Collaboration

#### 16. GitHub
- **Status:** ⚠️ Requires valid token
- **Env:** `GITHUB_TOKEN`
- **Tools:**
  - `search_repositories` / `search_code` / `search_issues` / `search_users`
  - `create_repository` / `fork_repository`
  - `get_file_contents` / `create_or_update_file` / `push_files`
  - `create_branch` / `list_commits`
  - `create_issue` / `list_issues` / `update_issue` / `add_issue_comment`
  - `create_pull_request` / `list_pull_requests` / `get_pull_request`
  - `merge_pull_request` / `create_pull_request_review`
- **Use Cases:** Repository management, PR workflows, issue tracking

---

### Kategori: Memory & AI

#### 17. Core Memory
- **Status:** ✅ Ready
- **Fungsi:** Persistent conversation memory
- **Tools:**
  - `memory_ingest` - Store conversation
  - `memory_search` - Search memories
  - `memory_about_user` - User profile
  - `get_labels` - List labels
  - `memory_get_documents` / `memory_get_document`
  - `initialize_conversation_session`
  - `get_integrations` / `get_integration_actions` / `execute_integration_action`
- **Use Cases:** Context persistence, user preferences, integrations

---

### Kategori: Tech Stack & Design

#### 18. StacksFinder
- **Package:** `@stacksfinder/mcp-server`
- **Status:** ✅ Partial (4 tools free)
- **Tools:**
  - `list_technologies` - List available techs
  - `analyze_tech` - 6-dimension analysis
  - `compare_techs` - Side-by-side comparison
  - `recommend_stack_demo` - Free daily recommendation
  - `recommend_stack` / `get_blueprint` / `create_blueprint` (Pro)

#### 19. Iconify
- **Package:** `pickapicon`
- **Status:** ✅ Ready
- **Tools:** `search_icons`, `get_icon`

---

### Kategori: Diagramming & UML

#### 20. Draw.io MCP
- **Package:** `drawio-mcp-server`
- **Status:** ✅ Ready
- **Fungsi:** Kontrol Draw.io via browser untuk Vibe Diagramming
- **Requirements:** Browser Extension (Chrome/Firefox)
- **Tools:**
  - **Inspection:** `get_diagram_info`, `get_selected_cells`, `get_cell_style`, `get_page_list`
  - **Modification:** `add_shape`, `add_edge`, `update_cell_style`, `delete_cells`, `set_cell_value`, `group_cells`, `ungroup_cells`, `move_cells`, `resize_cells`, `connect_cells`, `add_page`, `rename_page`, `delete_page`, `switch_page`, `set_page_size`, `export_diagram`
- **Use Cases:** Interactive diagram editing, flowcharts, architecture diagrams
- **Setup:** Install browser extension dari [Chrome Web Store](https://chrome.google.com/webstore/detail/drawio-mcp-extension/okdbbjbbccdhhfaefmcmekalmmdjjide) atau [Firefox Add-ons](https://addons.mozilla.org/en-US/firefox/addon/drawio-mcp-extension/)

#### 21. UML-MCP
- **Package:** `uml-mcp` (uvx)
- **Status:** ✅ Ready
- **Fungsi:** Generate UML diagrams langsung ke file gambar
- **Rendering Engines:** PlantUML, Kroki
- **Env:** `KROKI_SERVER`, `PLANTUML_SERVER`
- **Supported Diagrams:**
  - **UML:** Class, Sequence, Activity, Use Case, State, Component, Deployment, Object
  - **Other:** Mermaid, D2, Graphviz, ERD, BlockDiag, BPMN, C4 with PlantUML
- **Tools:**
  - `generate_uml` - Universal UML generator
  - `generate_class_diagram` - Class diagrams
  - `generate_sequence_diagram` - Sequence diagrams
  - `generate_activity_diagram` - Activity diagrams
  - `generate_usecase_diagram` - Use case diagrams
  - `generate_state_diagram` - State diagrams
  - `generate_component_diagram` - Component diagrams
  - `generate_deployment_diagram` - Deployment diagrams
  - `generate_erd` - Entity Relationship Diagrams
  - `generate_mermaid` - Mermaid diagrams
  - `generate_d2` - D2 diagrams
  - `generate_graphviz` - Graphviz diagrams
- **Use Cases:** UML documentation, ERD generation, system architecture diagrams

---

### Kategori: Office & Documents

#### 22. Excel
- **Status:** ✅ Ready
- **Tools:**
  - `create_workbook` / `create_worksheet`
  - `read_data_from_excel` / `write_data_to_excel`
  - `apply_formula` / `validate_formula_syntax`
  - `format_range` / `merge_cells` / `unmerge_cells`
  - `create_chart` / `create_pivot_table` / `create_table`
  - `copy_worksheet` / `delete_worksheet` / `rename_worksheet`
  - `insert_rows` / `insert_columns` / `delete_sheet_rows` / `delete_sheet_columns`
  - `get_workbook_metadata` / `get_data_validation_info`
- **Use Cases:** Excel automation, reports, data import/export

---

### Kategori: Container & DevOps

#### 23. Docker MCP
- **Package:** `docker-mcp` (uvx)
- **Status:** ✅ Ready
- **Fungsi:** Manage Docker containers dan compose stacks via AI
- **Requirements:** Docker Desktop harus running
- **Tools:**
  - `create-container` - Buat container standalone
  - `deploy-compose` - Deploy Docker Compose stack
  - `get-logs` - Ambil logs dari container
  - `list-containers` - List semua Docker containers
- **Use Cases:** Container management, deploy services, monitor logs, DevOps automation

---

## BAGIAN B: KIRO POWERS

### 1. Figma Power
- **URL:** `https://mcp.figma.com/mcp`
- **Status:** ✅ Ready
- **Keywords:** ui, design, code, layout, mockup, frame, component, frontend
- **Use Cases:** Design-to-code, design system rules, UI component mapping

### 2. Amazon Aurora PostgreSQL Power
- **Status:** ✅ Ready
- **Keywords:** aurora, postgresql, amazon, serverless, rds, database, aws
- **Use Cases:** Aurora PostgreSQL best practices, AWS database optimization

### 3. Power Builder
- **Status:** ✅ Ready
- **Keywords:** kiro power, power builder, build power, create power
- **Use Cases:** Create custom Kiro Powers, templates, validation

---

## Environment Variables

File `.env.mcp`:

```env
# Web Search (✅ Configured)
TAVILY_API_KEY=tvly-xxxxx
EXA_API_KEY=xxxxx
FIRECRAWL_API_KEY=xxxxx

# Version Control (⚠️ Need valid token)
GITHUB_TOKEN=ghp_xxxxx

# Database (⚠️ Need setup)
POSTGRES_CONNECTION_STRING=postgresql://user:pass@localhost:5432/simanis

# Optional
STACKSFINDER_API_KEY=xxxxx
SEARXNG_URL=http://localhost:8080
```

---

## Quick Reference

| Tool | Status | Best For |
|------|--------|----------|
| Desktop Commander | ✅ | Terminal, commands |
| Git | ✅ | Version control |
| Think Tool | ✅ | Complex reasoning |
| Sequential Thinking | ✅ | Multi-step problems |
| Playwright | ✅ | Browser automation |
| Filesystem | ✅ | File operations |
| Context7 | ✅ | Library docs |
| Telerik WPF | ✅ | WPF components |
| Tavily | ✅ | Web search |
| Exa | ✅ | AI search |
| Firecrawl | ✅ | Web scraping |
| Fetch | ✅ | Simple fetch |
| Core Memory | ✅ | Conversation memory |
| Excel | ✅ | Spreadsheet automation |
| Iconify | ✅ | UI icons |
| Draw.io MCP | ✅ | Interactive diagramming |
| UML-MCP | ✅ | UML diagram generation |
| Docker MCP | ✅ | Container management |
| StacksFinder | ✅ | Tech decisions |
| Figma Power | ✅ | Design-to-code |
| Aurora PostgreSQL | ✅ | AWS database |
| GitHub | ⚠️ | Repo management |
| Postgres MCP | ⚠️ | Database tuning |
| MySQL | ⚠️ | MySQL database |
| SearXNG | ⚠️ | Free search |

---

## Usage Tips untuk SIMANIS

1. **Development:** Desktop Commander + Git + Filesystem
2. **Testing UI:** Playwright untuk browser automation
3. **Research:** Tavily, Exa, atau Firecrawl
4. **Excel reports:** Excel MCP untuk import/export data
5. **Database:** Postgres MCP atau MySQL
6. **Code generation:** Context7 + Telerik WPF
7. **Design:** Figma Power untuk design-to-code
8. **Memory:** Core Memory untuk context persistence
9. **UML & Diagrams:** Draw.io MCP (interactive) atau UML-MCP (generate file)
10. **Docker:** Docker MCP untuk container management dan deployment
