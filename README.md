这是一份基于你提供的部署文档简练后的 **AI Agent 智能体 (v1.0.0)** 项目快速启动指南：

---

# AI Agent Station 部署手册

本项目是一个基于 **DDD (领域驱动设计)** 架构开发的 AI 智能体工作站，支持 MCP (Model Context Protocol) 协议、SSE 认证、知识库管理及多平台工具集成。

## 1. 核心功能更新
* **SSE 权限增强**：支持通过 `baseUri` 携带 `apikey` 或独立配置 `sseEndpoint` 进行身份验证。
* **Nginx 认证代理**：提供 Nginx 配置模版，通过校验请求参数实现对 MCP 服务的安全准入。
* **多工具集成**：内置微信公众号通知、CSDN 自动发帖、Playwright 联网搜索等 MCP 能力。

## 2. 快速部署步骤

### 第一步：环境配置
1.  **服务器准备**：建议 2C4G 配置，开放端口 `9000` (管理)、`8091` (API)、`8899` (MySQL)、`5050` (PGAdmin)。
2.  **修改配置**：
    * 编辑 `docker-compose-app.yml` 中的 `SPRING_OPENAI_API_KEY` 和 `BASE_URL`。
    * 在 `nginx/html/js/config.js` 中将 `BASE_URL` 指向你的服务器公网 IP。
3.  **三方 Key 准备**：
    * **微信**：获取测试号的 AppID、Secret、模板 ID 及用户 ID。
    * **CSDN**：从浏览器控制台获取个人账号的 Cookie。

### 第二步：启动服务
在 `docs/dev-ops` 目录下执行：
```bash
# 1. 启动基础环境 (MySQL, Redis, PostgreSql)
docker-compose -f docker-compose-environment-aliyun.yml up -d

# 2. 启动核心应用 (AI Station, MCP Servers)
docker-compose -f docker-compose-app.yml up -d
```

### 第三步：安装联网插件 (Playwright)
若需使用联网搜索功能，需进入容器手动初始化环境：
```bash
docker exec -it ai-agent-station-app /bin/bash
npx playwright@1.52.0 install-deps
npx playwright@1.52.0 install
```

## 3. 注意事项
* **数据库初始化**：首次启动后，需通过 8899 端口访问 MySQL 管理页面，核对并更新 `ai_client_model` 表中的模型接口地址。
* **模型推荐**：优先使用 `gpt-4o-mini` 或 `gpt-4.1`，兼顾响应速度与推理准确度。
* **网络环境**：联网插件可能受限于服务器网络环境，若无法访问外网检索，建议更换国内搜索 MCP 插件。
