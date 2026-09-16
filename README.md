# task-tree-orchestrator

大型任务规范化编排协议：以「任务结构树」为唯一事实源，把工作拆分为**管理者大模型**（统筹规划）与**执行者大模型**（落实呈递）双角色，通过轮次提示词与定时任务巡检完成对接。执行者可以是任意公司的模型（Claude / Codex / GPT 等），对接通道由人类配置。

## 核心三原则

- **树 = 唯一事实源**：谁干了什么、卡在哪、等什么，全部以树文件为准，不以聊天记录为准；
- **轮次 = 工作单位**：每轮一个枝、一个执行者、一份提示词、一次独立核验；单轮 ≤ 20 分钟；
- **Gate = 人类决策点**：全程 3 个左右 Gate，Gate 之间全自动流转，人类只在关键节点三选一（通过 / 需要修复 / 放弃）。

## 目录结构

```
task-tree-orchestrator/
├── SKILL.md                        # 主协议：角色定义、管理者协议、执行者协议、铁律、Gate 设计
└── references/
    ├── tree-template.md            # 树总览模板（branch 图/状态表/Gate 表/决策清单）
    ├── branch-template.md          # 枝文件模板（Goal/Tasks/Limitations/Results/BlockedBy/NeedsAuthorization）
    ├── round-prompt-template.md    # 轮次提示词模板（读树指令→任务→硬边界→环境坑→收尾核验→中断恢复→呈递格式）
    ├── worker-protocol.md          # 执行者协议全文（写入树，每轮提示词要求执行者读）
    └── scheduler-protocol.md       # 定时任务巡检协议（第 0 步铁律→状态判定规则①②③→核验→投递→回写→呈递）
```

## 安装

### Claude Code

1. 将本目录复制到你的 skills 目录（或直接以仓库形式 clone 过去）：
   ```bash
   # macOS / Linux
   mkdir -p ~/.claude/skills && cp -r . ~/.claude/skills/task-tree-orchestrator
   # Windows（示例）
   mkdir "%USERPROFILE%\.claude\skills" && xcopy /E /I . "%USERPROFILE%\.claude\skills\task-tree-orchestrator"
   ```
2. 重启 Claude Code 会话，skill 即注册生效。

### Codex（或其他 agent 平台）

1. 将本目录内容放入该平台的 skill 目录；
2. 由于 `description` 已附英文触发句，支持语义匹配的平台可直接按描述触发。

## 给 Agent 的安装提示（复制这段话发给你的 agent 即可）

> 请把我提供的 `task-tree-orchestrator` 目录安装为本机的 skill：将整个目录（含 `SKILL.md` 与 `references/` 五个模板文件）复制到你的 skills 目录（Claude Code 为 `~/.claude/skills/` 或平台指定的 skill 目录），保持目录名 `task-tree-orchestrator` 不变，然后重启会话使其生效。不要修改目录内任何文件的内容。

## 适用与不适用

- **适用**：大型、多模块、多轮次任务；人类指定了执行者模型（可能是别的公司/别的产品的模型）；需要对既有大型任务建立结构树规范化管理。
- **不适用**：单文件小修小补、一次性问答、无多轮协作需求的普通编码任务。

## License

MIT
