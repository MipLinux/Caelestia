# 协作说明

本仓库是 MipLinux 维护的 Caelestia dotfiles fork：Hyprland / Wayland 桌面及其周边组件的配置。
它不包含 shell 本体，也不包含安装器 / CLI。

| 仓库 | 内容 |
|---|---|
| [`MipLinux/Caelestia-dotfiles`](https://github.com/MipLinux/Caelestia-dotfiles) | 本仓库：dotfiles 与主题配置 |
| [`MipLinux/Caelestia-shell`](https://github.com/MipLinux/Caelestia-shell) | 顶栏、面板、启动器等 shell 本体（fork） |
| [`MipLinux/Caelestia-cli`](https://github.com/MipLinux/Caelestia-cli) | `caelestia` 安装 / 更新命令（fork） |
| [`caelestia-dots/shell`](https://github.com/caelestia-dots/shell) | shell 上游 |
| [`caelestia-dots/cli`](https://github.com/caelestia-dots/cli) | CLI 上游 |
| [`MipLinux/MipLinux`](https://github.com/MipLinux/MipLinux) | 发行版构建仓库；桌面配置取自本仓库 |

---

## 先读这个

| 顺序 | 文档 | 内容 |
|---|---|---|
| 1 | [`README.md`](../README.md) | 安装、更新、用户配置覆盖层 |
| 2 | [`manifest.toml`](../manifest.toml) | 组件、包、`src → dest` 的映射 |
| 3 | [`.github/review/README.md`](review/README.md) | 自动审查流程与所需 secrets / variables |
| 4 | [`.github/pull_request_template.md`](pull_request_template.md) | PR 必须填的三节 |

**不要**把本仓库当成 shell 仓库来改。顶栏、面板、启动器的行为问题请提到 [`Caelestia-shell`](https://github.com/MipLinux/Caelestia-shell)。

---

## 工作方式

### 一条工作 = 一个 issue

功能、重构、需要跨文件协调的改动，先在 [Issues](https://github.com/MipLinux/Caelestia-dotfiles/issues?q=label%3Atask)
开一条带 `task` label 的工作 issue，写清「目标 / 实现 / 验收 / 回报 / 依赖」。
`assignee` 就是受理人；没有对应 issue 的改动，维护者可以要求先补。

小修、纯文档改动可以直接开 PR，但仍要走模板里的「验证方式」。

### 上游同步

`main` 跟踪上游，只保留最小的 MipLinux 文档 / 许可声明；功能改动放 `mip` 分支 / tag，PR 描述里记录上游 base commit。
不重写历史；dots / shell / cli 与 Quickshell 的 pin 一起对齐，不单独升级其中一个。

### 分支

功能 / 行为 / 配置改动从 `mip` 分支或维护者指定的 `mip-*` tag 切；只有文档改动从 `main` 切。

```bash
# 功能：从 mip 切
git fetch origin
git switch mip
git pull --ff-only
git switch -c feat/<主题>     # 修 Bug 用 fix/<主题>

# 功能：从维护者指定的 mip-* tag 切
git fetch --tags origin
git switch -c feat/<主题> <mip-tag>

# 仅文档 / 纯文案
git switch main
git pull --ff-only
git switch -c docs/<主题>
```

`mip` 不存在时，向维护者要指定的 tag；不要从 `main` 直接开功能分支。
分支不在目标分支上、或拉取不是快进 → 停下来问维护者，**不要**自行 `merge` / `rebase` / `reset --hard`。

### 提交信息

第一行用 `组件: 做了什么`，例如：

```
hypr: 缺少用户覆盖文件时回退到默认变量

原先直接 load 用户文件，文件不存在就报错。改为先判断存在性。
```

- 组件名尽量与顶层目录 / `manifest.toml` 的 `name` 一致：`hypr`、`fish`、`foot`、`fastfetch`、`btop`、
  `firefox`、`thunar`、`micro`、`nvim`、`vscode`、`zed`、`zen`、`spicetify`、`starship`、`uwsm`、`manifest`。
- 一次提交只做一件事；多个改动时，标题写影响最大的那个，其余放正文。

### PR

PR 描述按模板写三件事：

1. **改了什么** —— 涉及的组件 / 文件
2. **为什么** —— 解决什么问题，关联 issue 写 `Closes #N`
3. **验证方式** —— 只写真实跑过的命令与结果；没跑的写「未验证」

`main` 受保护：PR 需要至少 1 个 approving review，且必须包含 code owner（`@LaT-SKY`）的审核。

---

## 提交前自检

先做静态检查（不依赖图形会话）：

```bash
git diff --check
luac -p hypr/hyprland.lua hypr/variables.lua
jq empty zed/settings.json zed/keymap.json vscode/settings.json vscode/keybindings.json
bash -n firefox/init_firefox.sh
```

再按改动范围做实际验证：

- **Hyprland**：在测试用户里重登或 `hyprctl reload`，确认 `hyprctl configerrors` 无输出；
- **终端 / shell**：开一个新的 fish 会话，确认提示符、别名、环境变量生效；
- **浏览器主题**：重启 firefox / zen，确认 `userChrome.css` 与 `user.js` 生效；
- **编辑器 / 文件管理器**：打开对应组件，确认配置被读取；
- **manifest / 安装流程**：在测试用户执行一次 `caelestia install` 或 `caelestia update`。

**不要**在没有测试用户隔离的情况下直接覆盖自己的工作配置；也不要把「静态检查通过」写成「已验证」。

---

## 自动审查

仓库启用了四条 workflow：

| workflow | 触发 | 作用 |
|---|---|---|
| `auto-assign.yml` | issue / PR 创建 | 自动分给维护者 |
| `ai-summary.yml` | issue 创建 / 编辑 | 受理前质检、所有权雷达、查重、引用有效性 |
| `pr-acceptance.yml` | PR 打开 / 标记 ready | 把 issue 的「验收」与 PR 的「验证方式」对账 |
| `pr-review.yml` | 维护者评论 `/mipl-review`（或 `/caelestia-review`） | 严格型代码与红线审查 |

判据在 [`.github/review/`](review/)；所需 secrets / variables 见 [review/README.md](review/README.md)。
自动审查只描述事实，不出「通过 / 不通过」，也不拦合并 —— 合并判断仍由人做。

---

## 报告问题

- **配置 / 脚本 Bug** → [`bug-report.yml`](ISSUE_TEMPLATE/bug-report.yml)
- **文档与实际不符** → [`documentation.yml`](ISSUE_TEMPLATE/documentation.yml)
- **新功能 / 改进** → [`feature-request.yml`](ISSUE_TEMPLATE/feature-request.yml)
- **需要跨文件协调的工作** → [`task.yml`](ISSUE_TEMPLATE/task.yml)（label `task`）
- **安全漏洞** → [MipLinux SECURITY.md](https://github.com/MipLinux/.github/blob/main/SECURITY.md)，不要在公开 Issue 里披露

---

## 许可

本仓库以 **GPL-3.0-only** 发布（上游 Caelestia 的 LICENSE 为 GPLv3），全文见 [LICENSE](../LICENSE)，版权与修改声明见 [NOTICE](../NOTICE.md)。提交即表示你同意以同一许可证发布你的贡献；修改上游文件时保留原版权声明并标明改动。
