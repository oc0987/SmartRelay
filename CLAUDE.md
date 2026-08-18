# SmartRelay 项目说明

商用级物联网项目（ESP32 系列芯片 + 云服务 + 用户端）。本文件是团队协作的入口说明。

## 团队结构

本项目由五个 Claude Code subagent 协作开发（位于 `.claude/agents/`）：

| 角色 | Agent 名称 | 职责 | 技术栈 |
|---|---|---|---|
| 系统架构师 | system-architect | 项目拆解、任务分派、风险把控、团队协调 | 三端通识 + ESP32 物联网 |
| UI 原型设计师 | ui-designer | 高保真 UI 原型（小程序/用户端/后台） | HTML + Tailwind CSS |
| 服务端工程师 | server-engineer | 服务端、数据库、MQTT 数据流 | Python + MySQL + MQTT |
| 前端工程师 | frontend-engineer | 前端页面 + 小程序 | Vue3 + 微信小程序 |
| 嵌入式工程师 | embedded-engineer | 设备端固件 | ESP32 + ESP-IDF V5.4.4 + MQTT |

## 协作流程

1. 用户提出需求 → system-architect 拆解任务、识别风险并分派。
2. UI 需求 → ui-designer 产出高保真原型 → frontend-engineer 落地实现。
3. 服务端 → server-engineer 设计数据库/API/MQTT 数据流 → 同步给 frontend-engineer 与 embedded-engineer。
4. 固件 → embedded-engineer 实现并编译验证 → 按 server-engineer 的 MQTT 规范对接。
5. system-architect 审查产出并汇总进展与风险。

## 关键约定

- **ESP-IDF 固定 V5.4.4**，版本变更必须先确认。
- 商用项目必须考虑：设备鉴权、TLS 加密、数据安全、OTA 升级。
- 跨端契约（MQTT topic/payload、REST 接口）以 server-engineer 的输出为权威来源，变更需同步所有受影响方。
- UI 原型（design/prototypes/）仅为视觉参考，正式前端代码由 frontend-engineer 用项目样式体系实现。
