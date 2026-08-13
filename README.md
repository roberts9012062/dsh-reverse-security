<p align="center">
  <h1 align="center">dsh-reverse-security</h1>
  <h3 align="center">基于 reverse-skill 的 DeepSeek Harness 逆向 / 渗透 / 安全研究 Agent 预设</h3>
</p>

<p align="center">AI 自动路由 · 授权门禁 · 按需自举工具链 · 经验库自进化</p>

<p align="center">
  <a href="#这是什么">关于</a> ·
  <a href="#特性">特性</a> ·
  <a href="#安装">安装</a> ·
  <a href="#使用说明">使用说明</a> ·
  <a href="#技能清单">技能清单</a> ·
  <a href="#致谢">致谢</a> ·
  <a href="#社区">社区</a> ·
  <a href="#许可证">许可证</a>
</p>

---

## 这是什么

`dsh-reverse-security` 是 [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness)（DSH）的一个 **Agent 预设（preset）**：它把 [reverse-skill](https://github.com/zhaoxuya520/reverse-skill) 的 40+ 逆向 / 渗透 / 安全技能路由能力，打包成 DSH 上一个即插即用的安全研究 Agent。

当你在 DSH 里遇到 APK 逆向、二进制分析、JS 前端加密、协议还原、CTF、渗透测试、恶意样本等任务时，这个 Agent 会**先路由到正确的方法论，再调用本机工具执行**，而不是盲目猜命令。

```
用户任务
  → 路由（MASTER-ROUTING / master-route.ps1 / routing.json）
  → case-init 授权门禁（scope.md，auth 未 granted 禁止对目标 ACT）
  → PRIMARY SKILL.md（领域方法论 + 工具链）
  → tool-index 真实工具路径 → 缺工具 bootstrap
  → Evidence → Finding → Path 证据链
  → docs-generator 报告 + field-journal 经验回写
```

## 特性

- **40+ 领域技能**：APK / 移动端 / iOS / JS / .NET / IDA / Ghidra / r2 / 固件 / EDR / 协议 / 云 / AD / 取证 / 代码审计 / 威胁狩猎 / 工控 / 无线 / 硬件 / LLM / API / 供应链 / 恶意样本 / pwn 等
- **自动路由**：`MASTER-ROUTING.md` + `config/routing.json`（单一事实源）按「目标类型 / 用户意图 / 工具链」三轴路由，42 条快路径规则
- **授权门禁**：`case-init` 生成 `scope.md`，`auth.status=granted` 前禁止对目标 ACT；只做授权范围内的安全工作
- **按需自举工具链**：缺工具 → `bootstrap-reverse.ps1`（仅清单能力，不猜路径）
- **经验库自进化**：`field-journal/` 先例库 + 任务完成 Checklist（报告 + 图表 + journal 回写）
- **完整证据链**：Evidence → Finding → Path，结论可追溯、可审查（`case-review`）

## 安装

### 前置要求

- 一个可用的 DeepSeek Harness（DSH）环境
- Windows 推荐（自带 PowerShell 工具链脚本）；Linux / macOS 亦可（内置 bash 脚本）

### 步骤

1. 克隆仓库：

   ```bash
   git clone https://github.com/roberts9012062/dsh-reverse-security.git
   ```

2. 安装为 DSH 预设。把目录放进 DSH 的用户预设根 `${DSH_HOME:-$HOME/.dsh}/.agent-presets/`：

   Windows PowerShell：

   ```powershell
   $root = if ($env:DSH_HOME) { $env:DSH_HOME } else { "$HOME\.dsh" }
   New-Item -ItemType Directory -Force -Path "$root\.agent-presets" | Out-Null
   Copy-Item -Recurse -Force .\dsh-reverse-security "$root\.agent-presets\dsh-reverse-security"
   ```

   Linux / macOS：

   ```bash
   ROOT="${DSH_HOME:-$HOME/.dsh}"
   mkdir -p "$ROOT/.agent-presets"
   cp -r dsh-reverse-security "$ROOT/.agent-presets/dsh-reverse-security"
   ```

3. （可选，推荐）生成**本机工具索引**，让 Agent 拿到真实工具路径（`skills/tool-index.md` 是每台机器不同的生成文件）：

   ```powershell
   powershell -NoProfile -ExecutionPolicy Bypass -File "<预设目录>/skills/scripts/refresh-tool-index.ps1"
   ```

## 使用说明

1. 在 DSH 里新建会话，预设选择 **「逆向安全研究」**（目录 id 为 `dsh-reverse-security`）。
2. 直接描述任务，例如：

   - 「分析这个 APK 的签名校验逻辑」
   - 「抓包还原这个 App 的加密协议」
   - 「这个二进制有反调试，帮我定位并绕过」
   - 「对这个授权目标做一次 Web 渗透测试」
   - 「这份样本疑似恶意软件，帮我做行为分析」

3. Agent 会按行为链自动执行，并在每个阶段结束给出 3–6 个编号的「下一步」选项，由你选择方向（至少包含「导出报告」和「继续深入 / 换方法」）。

> **安全边界**：本预设只做授权范围内的安全工作（书面授权 / CTF 靶场 / 自有样本 / 公开漏洞研究）。对未授权目标动手会直接拒绝。

## 技能清单

| 领域 | 技能 |
| --- | --- |
| 总路由 | `reverse-skill-router` |
| 二进制 / 逆向 | `reverse-engineering` · `ida-reverse` · `ghidra-reverse` · `radare2` · `go-rust-reverse` · `dotnet-reverse` · `dsl-vm-reverse` · `binary-diff` · `patch-diff-exploit` · `pwn-chain` |
| 移动端 | `apk-reverse` · `mobile-reverse` · `macos-reverse` |
| 前端 / 客户端 | `js-reverse` · `browser-automation` · `browser-extension-reverse` · `thick-client` |
| 渗透 / 攻击 | `pentest-tools` · `src-hunter` · `attack-chain` · `edr-bypass-re` · `windows-ad` · `cloud-k8s` · `wifi-wireless` · `ot-ics` · `hardware-security` · `radio-sdr` |
| 应用 / 服务安全 | `api-security` · `database-security` · `email-security` · `identity-federation` · `llm-security` · `supply-chain-security` · `code-audit` |
| 协议 / 固件 | `protocol-reverse` · `firmware-pentest` |
| 分析 / 防御 | `malware-analysis` · `digital-forensics` · `threat-hunting` |
| 元技能 | `docs-generator` · `diagram-generator` · `case-review` |

## 目录结构

```
├── agent.cordis.yml   # DSH 预设组合（standard 底座 + 安全 persona + skill-filesystem）
├── preset.yml         # 预设元数据（名称 / 描述）
├── RULES.md           # 行为链规则（唯一真相源）
└── skills/
    ├── SKILL.md              # reverse-skill-router 总路由
    ├── <技能>/SKILL.md        # 40+ 领域技能
    ├── scripts/              # 路由 / 门禁 / 自举 / 索引脚本
    ├── ops/                  # 作战契约（scope / 证据链 / 角色 / 时间线）
    ├── field-journal/        # 经验库（先例 + 模板 + 种子）
    ├── config/               # routing.json 路由事实源
    ├── references/           # 社区技能对照
    └── tests/                # 路由基准
```

## 致谢

本项目是 [reverse-skill](https://github.com/zhaoxuya520/reverse-skill)（作者 [@zhaoxuya520](https://github.com/zhaoxuya520)）的 **DeepSeek Harness 移植与适配**。`skills/` 目录下的领域技能、路由系统、脚本、作战契约（ops）与经验库（field-journal）均源自 reverse-skill。

由衷感谢 reverse-skill 的作者 [@zhaoxuya520](https://github.com/zhaoxuya520) 及其社区贡献者，打造了这套优秀的逆向 / 渗透 / 安全技能路由体系，让 AI Agent 能在安全研究场景下「先路由、后动手」。本项目仅做了平台适配（移植到 DSH 的 Agent 预设形态），核心方法论与技能内容归功于 reverse-skill。

第三方内容归属详见 [NOTICE.md](NOTICE.md)。

## 社区

本项目已获 [LINUX DO 社区](https://linux.do/) 认可与支持，欢迎到 LINUX DO 一起交流：

- 社区：[LINUX DO](https://linux.do/)
- 问题反馈：[GitHub Issues](https://github.com/roberts9012062/dsh-reverse-security/issues)

## 许可证

本项目采用 [MIT License](LICENSE)。

`skills/` 目录内容源自 [reverse-skill](https://github.com/zhaoxuya520/reverse-skill)，其版权归原作者 [zhaoxuya520](https://github.com/zhaoxuya520) 所有，遵循 [MIT License](https://github.com/zhaoxuya520/reverse-skill/blob/main/LICENSE)。
