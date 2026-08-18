# SmartRelay 团队 Agent 体系设计规格

日期：2026-08-18
状态：已批准

## 背景

SmartRelay 是商用级物联网项目（ESP32 系列芯片 + 云服务 + 用户端）。为支撑多岗位并行开发，在 `.claude/agents/` 下搭建五个分工明确的 Claude Code subagent，由系统架构师统一调度。

## 团队角色

| 角色 | Agent 文件 | 职责 | 技术栈 |
|---|---|---|---|
| A. SystemArchitect | `system-architect.md` | 项目拆解、任务分派、风险把控、团队协调、产出汇总 | 前端/服务端/嵌入式三端通识，ESP32 物联网 |
| B. UIDesigner | `ui-designer.md` | 高保真 UI 交互原型（小程序/网站用户端/网站后台） | HTML + Tailwind CSS |
| C. ServerEngineer | `server-engineer.md` | 服务端开发、数据库设计、数据交流流程设计 | Python、MySQL、MQTT |
| D. FrontEndEngineer | `frontend-engineer.md` | 前端页面与小程序开发，将 UIDesigner 原型落地为正式代码 | Vue3、微信小程序 |
| E. EmbeddedEngineer | `embedded-engineer.md` | 设备端固件开发，对接服务端与前端 | ESP32、ESP-IDF **V5.4.4**、MQTT |

## 文件结构

```
.claude/
└── agents/
    ├── system-architect.md
    ├── ui-designer.md
    ├── server-engineer.md
    ├── frontend-engineer.md
    └── embedded-engineer.md
CLAUDE.md   # 项目说明：团队结构、协作流程
```

## 权限设计（tools）

| Agent | Tools | 理由 |
|---|---|---|
| SystemArchitect | `*`（全部） | 需要 Agent 工具调度其他四位、Bash 执行检查、全库读写 |
| UIDesigner | Read/Write/Edit/Glob/Grep/Bash | 编写 HTML 原型文件、启动本地预览服务器 |
| ServerEngineer | 读写 + Bash + WebFetch/WebSearch | Python/MySQL 开发、运行服务、查询文档 |
| FrontEndEngineer | 读写 + Bash | Vue3/小程序开发、npm 构建 |
| EmbeddedEngineer | 读写 + Bash + WebFetch/WebSearch | `idf.py` 编译烧录、查询 ESP-IDF V5.4.4 文档 |

所有 agent 均带 Explore 能力（代码库搜索）。只读工具默认放行。

## 协作流程

```
用户需求
   │
   ▼
SystemArchitect（拆解任务 → 识别风险 → 分派）
   ├──► UIDesigner       产出 HTML+Tailwind 高保真原型
   ├──► ServerEngineer   设计数据库/接口/MQTT 数据流
   ├──► FrontEndEngineer 原型落地 Vue3/小程序 + 对接接口
   └──► EmbeddedEngineer ESP-IDF 固件 + MQTT 对接服务端
   │
   ▼
SystemArchitect 审查各端产出，汇总风险与进展同步用户
```

## 关键设计决策

1. **SystemArchitect 是唯一有调度权的 agent**：其他工程师只做本职，避免多头指挥。
2. **UIDesigner 与 FrontEndEngineer 职责分离**：原型为高保真参考，正式项目由前端用 Vue3/小程序重新实现。
3. **EmbeddedEngineer 固定使用 ESP-IDF V5.4.4**：版本写入提示词，防止版本漂移。

## 各 agent 提示词要点

每个 agent 文件包含：角色定位、专业技能、职责范围、工作流程、交付物格式、验收标准、对接方说明。SystemArchitect 额外包含：任务拆解方法、风险识别清单、分派与汇总规则。
