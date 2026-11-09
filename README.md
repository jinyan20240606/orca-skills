# Orca 内置 Skill 文本（学习用副本）

本目录是从本机安装的 Orca（macOS `/Applications/Orca.app`）中**还原**出来的全部内置 skill 文本，
用于研究和学习，不是从上游 git 仓库 clone 的源码。

## 目录结构

```
orca-skills/
├── README.md
├── source-manifest.json
├── computer-use/
│   └── SKILL.md
├── linear-tickets/
│   └── SKILL.md
├── orca-cli/
│   ├── SKILL.md
│   └── references/{automations,browser,publishing}.md
├── orca-emulator/
│   └── SKILL.md
├── orca-emulator-android/
│   └── SKILL.md
├── orca-linear/
│   └── SKILL.md
├── orca-per-workspace-env/
│   ├── SKILL.md
│   └── references/{docker-ssh,failure-modes,provider-vercel,ssh-host,windows-scripts}.md
└── orchestration/
    ├── SKILL.md
    └── references/{coordinator-loop,legacy-contract-migration,low-level-topology,
                    messaging-and-gates,placement-and-remote,
                    recovery-and-cleanup,worker-contract}.md
```

## 八个内置 skill 各自干什么

| Skill | 作用 | 行数 |
|---|---|---|
| `computer-use` | 通过 `orca computer` 驱动**本机可见窗口**的 GUI：辅助功能树、点击、输入、菜单、对话框、截图。仅当 shell/文件/HTTP 等程序化路径无法完成时才用。 | 153 |
| `linear-tickets` | 通过 Orca CLI 做 Linear 工单：从关联 issue 开工、完成后回写 PR/MR 链接和评论、推进工作流状态、搜索、创建子工单。**`orca-linear` 的遗留别名。** | 165 |
| `orca-cli` | **操作层主手册**：worktree、folder context、terminal、repo、automation、artifact、skill 分享、内置浏览器、终端读写与等待。 | 267 |
| `orca-emulator` | macOS 上驱动 iOS Simulator：点击、手势、输入、硬件键、旋转、辅助功能树；设备画面在 Orca 模拟器面板里。 | 104 |
| `orca-emulator-android` | 通过 adb 驱动 Android 设备/模拟器：启动 AVD、点击滑动、输入、旋转、安装启动、运行时权限、辅助功能树、logcat。 | 118 |
| `orca-linear` | **同 `linear-tickets` 的现行名称**；正文完全一致，仅 name/description/标题和一段遗留说明不同。 | 162 |
| `orca-per-workspace-env` | 配置/调试每个 workspace 的**一次性运行时**（云沙箱、VM、SSH 主机、本地容器）：`orca.yaml` 的 `environmentRecipes`、provider 生命周期脚本、`orca vm recipe doctor`。 | 376 |
| `orchestration` | **协作层协议**：Run / Task / Dispatch 落库、线程消息、阻塞 ask/reply、DAG、决策闸门、worker_done/escalation 等待、协调者循环。 | 199 |

## 来源与版本

Orca 构建时把上游仓库的 `skills/<name>/SKILL.md` 和 `references/*.md`
内联进 `out/cli/bundled-skill-guides.js`，再由 `orca skills get` 读出。
因此本机没有原始的 skill 目录。

| Skill | 上游路径 | revision |
|---|---|---|
| computer-use | `skills/computer-use` | 9 |
| linear-tickets | `skills/linear-tickets` | 11 |
| orca-cli | `skills/orca-cli` | 37 |
| orca-emulator | `skills/orca-emulator` | 8 |
| orca-emulator-android | `skills/orca-emulator-android` | 6 |
| orca-linear | `skills/orca-linear` | 9 |
| orca-per-workspace-env | `skills/orca-per-workspace-env` | 6 |
| orchestration | `skills/orchestration` | 29 |

完整的 gitTreeSha / packageDigest 见 `source-manifest.json`。

## 还原方式

```bash
orca skills list                     # 列出所有内置 topic
orca skills get <topic> > SKILL.md
orca skills get <topic> --references # 列出 references
orca skills get <topic> --reference <name> > references/<name>.md
```

`orca skills get` 只读取随二进制发布的本地内容，不联网、不依赖 Orca 运行时。

## 如何刷新

升级 Orca 后重跑上面的命令。也可用 `orca skills get <topic> --full` 一次性查看主文档 + 全部 references。

## 注意

- 文本版权归 Orca 项目所有，此处仅作个人学习副本，请勿再分发。
- 还原目录不等于上游完整源码树；上游可能还有生成脚本、测试、schema 等未随二进制发布的内容。
- `~/.agents/skills/` 下已安装的同名文件可能是**发现用 stub**（只负责把 agent 引到 `orca skills get`），不是完整指南。
- `computer-use` 只有一份 guide；它的详细操作仍靠 `orca computer --help` 和其他内置文档。
