# @markfu909/overleaf-mcp

[English](README.md) | [中文](README.zh-CN.md)

一个 MCP (Model Context Protocol) 服务器，通过 Git 集成提供对 Overleaf 项目的访问。允许 Claude 和其他 MCP 客户端读取 LaTeX 文件、分析文档结构并从 Overleaf 项目中提取内容。

## 功能特性

- **文件管理**: 列出和读取 Overleaf 项目中的文件
- **文档结构**: 解析 LaTeX 章节和子章节
- **内容提取**: 按标题提取特定章节
- **项目概览**: 获取项目状态和结构概览
- **多项目支持**: 管理多个 Overleaf 项目

## 快速开始 (npx)

最简单的使用方式是通过 npx。在 Claude Desktop 配置中添加：

```json
{
  "mcpServers": {
    "overleaf": {
      "command": "npx",
      "args": ["-y", "@markfu909/overleaf-mcp"]
    }
  }
}
```

然后在用户主目录创建配置文件 `~/.overleaf-mcp/projects.json`：

```json
{
  "projects": {
    "default": {
      "name": "我的论文",
      "projectId": "你的OVERLEAF项目ID",
      "gitToken": "你的OVERLEAF_GIT令牌"
    }
  }
}
```

## 手动安装

1. 克隆仓库：
   ```bash
   git clone https://github.com/tianhao909/overleaf-mcp.git
   cd overleaf-mcp
   ```

2. 安装依赖：
   ```bash
   npm install
   ```

3. 设置项目配置：
   ```bash
   cp projects.example.json projects.json
   ```

4. 编辑 `projects.json`，填入你的 Overleaf 凭证：
   ```json
   {
     "projects": {
       "default": {
         "name": "我的论文",
         "projectId": "你的OVERLEAF项目ID",
         "gitToken": "你的OVERLEAF_GIT令牌"
       }
     }
   }
   ```

## 获取 Overleaf 凭证

1. **Git 令牌**: 
   - 进入 Overleaf 账户设置 → Git Integration
   - 点击 "Create Token"

2. **项目 ID**: 
   - 打开你的 Overleaf 项目
   - 在 URL 中找到: `https://www.overleaf.com/project/[项目ID]`

## Claude Desktop 配置

将以下内容添加到 Claude Desktop 配置文件：

**Windows**: `%APPDATA%\Claude\claude_desktop_config.json`
**macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
**Linux**: `~/.config/claude/claude_desktop_config.json`

```json
{
  "mcpServers": {
    "overleaf": {
      "command": "node",
      "args": [
        "/path/to/OverleafMCP/overleaf-mcp-server.js"
      ]
    }
  }
}
```

配置后重启 Claude Desktop。

## 可用工具

### `list_projects`
列出所有已配置的项目。

### `list_files`
列出项目中的文件（默认: .tex 文件）。
- `extension`: 文件扩展名过滤器（可选）
- `projectName`: 项目标识符（可选，默认为 "default"）

### `read_file`
读取项目中的特定文件。
- `filePath`: 文件路径（必填）
- `projectName`: 项目标识符（可选）

### `get_sections`
获取 LaTeX 文件中的所有章节。
- `filePath`: LaTeX 文件路径（必填）
- `projectName`: 项目标识符（可选）

### `get_section_content`
获取特定章节的内容。
- `filePath`: LaTeX 文件路径（必填）
- `sectionTitle`: 章节标题（必填）
- `projectName`: 项目标识符（可选）

### `status_summary`
获取项目状态概览。
- `projectName`: 项目标识符（可选）

## 使用示例

```
# 列出所有项目
使用 list_projects 工具

# 获取项目概览
使用 status_summary 工具

# 读取 main.tex 文件
使用 read_file，参数 filePath: "main.tex"

# 获取引言章节
使用 get_section_content，参数 filePath: "main.tex"，sectionTitle: "Introduction"

# 列出文件中的所有章节
使用 get_sections，参数 filePath: "main.tex"
```

## 多项目使用

在 `projects.json` 中添加多个项目：

```json
{
  "projects": {
    "default": {
      "name": "主论文",
      "projectId": "project-id-1",
      "gitToken": "token-1"
    },
    "paper2": {
      "name": "第二篇论文", 
      "projectId": "project-id-2",
      "gitToken": "token-2"
    }
  }
}
```

然后在工具调用中指定项目：
```
使用 get_section_content，参数 projectName: "paper2", filePath: "main.tex", sectionTitle: "Methods"
```

## 文件结构

```
OverleafMCP/
├── overleaf-mcp-server.js    # 主 MCP 服务器
├── projects.json             # 你的项目配置（已加入 gitignore）
├── projects.example.json     # 示例配置
├── package.json              # 依赖配置
├── README.md                 # 英文文档
└── README.zh-CN.md           # 中文文档
```

## 安全说明

- `projects.json` 已加入 gitignore 以保护你的凭证
- 不要提交真实的项目 ID 或 Git 令牌
- 使用提供的 `projects.example.json` 作为模板

## 致谢

本项目 fork 自 [mjyoo2/OverleafMCP](https://github.com/mjyoo2/OverleafMCP)。感谢原作者创建了这个优秀的 MCP 服务器。

**本 fork 的改进：**
- 添加 npm 包支持（可通过 npx 安装）
- 添加中文文档
- 支持多配置文件路径
- 各种部署优化改进

## 许可证

MIT License
