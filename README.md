# caelestia

[English](README.en.md) | 简体中文

这是 Caelestia dotfiles 的主仓库，包含各类应用的用户配置。

> [!IMPORTANT]
> 本仓库旧的 `install.fish` 脚本已移除，改用 [CLI][cli-repo] 的安装命令。
>
> 如果你用旧脚本安装过，请先更新 CLI，再运行安装命令完成迁移。

> [!IMPORTANT]
> Hyprland 配置已切换到 Lua！
> 如果你有自定义的 `~/.config/caelestia/hypr-user.conf`
> 或 `~/.config/caelestia/hypr-vars.conf`，请手动转换为 Lua，
> 或使用可用的在线转换器。
>
> 这些文件的用法见[配置](#配置)一节。

## 安装（Arch Linux）

从 AUR 安装 CLI，然后运行 `caelestia install`。

例如：

```sh
paru -S caelestia-cli
caelestia install
```

### 手动安装

克隆本仓库，按 [manifest](/manifest.toml) 安装你想启用组件所需的全部软件包，
再复制这些组件对应的条目。

例如 hyprland 组件：

```sh
git clone https://github.com/caelestia-dots/caelestia.git
cd caelestia
sudo pacman -S --needed hyprland xdg-desktop-portal-hyprland xdg-desktop-portal-gtk ttf-jetbrains-mono-nerd
mkdir -p $XDG_CONFIG_HOME/hypr
cp -r hypr/. $XDG_CONFIG_HOME/hypr/
```

## 更新

运行 `caelestia update` 完成系统更新并更新 dotfiles。

## 使用

> [!NOTE]
> 这套配置目前不包含登录管理器。因此，除非你打算从 TTY 登录，
> 否则需要自行安装一个登录管理器。我推荐
> [`greetd`](https://sr.ht/~kennylevinsen/greetd) 搭配
> [`tuigreet`](https://github.com/apognu/tuigreet)；当然，你也可以使用任何喜欢的登录管理器。

## 配置

> [!CAUTION]
> 不要修改 `~/.config/hypr/` 里的任何文件，否则更新 dotfiles 时会产生冲突。
>
> 个人改动应写在 `~/.config/caelestia/hypr-user.lua` 或 `hypr-vars.lua` 中，
> 安装 / 更新流程永远不会修改这两个文件。把改动写进 `~/.config/hypr/`
> 会阻止后续更新生效，之后每次手动更新都要处理冲突。

### hypr-vars.lua

大多数 Hyprland 默认值都可以通过在 `~/.config/caelestia/hypr-vars.lua` 中覆盖变量来修改。
你可以用它设置默认应用、键位、鼠标指针、窗口装饰等。
所有可用变量及其默认值见 [`variables.lua`](hypr/variables.lua)。

`hypr-vars.lua` 示例：

```lua
return {
  -- 修改默认应用：
  browser          = "zen-browser",    -- 将默认浏览器改为 Zen
  editor           = "code",           -- 将默认编辑器改为 Code

  -- 修改窗口 / 装饰属性：
  blurEnabled      = false,            -- 关闭窗口模糊
  windowBorderSize = 3,                -- 增大窗口边框（默认：1）

  -- 修改键位：
  kbTerminal       = "SUPER + Return", -- 修改打开终端的键位
}
```

### hypr-user.lua

你可以把任意自定义 Hyprland 配置放进 `~/.config/caelestia/hypr-user.lua`。
该文件会在 Hyprland 加载流程的最后被读取，因此你可以在预制配置之外
自定义 Hyprland 行为，包括显示器布局、额外键位和窗口规则。
如果改动的是 dotfiles 已经管理的选项（例如键位、窗口边框），
优先使用 `hypr-vars.lua`；如果配置不在 dotfiles 覆盖范围内，或需要在变量之外管理，
则写到 `hypr-user.lua`。
更多 Hyprland 配置说明见官方 [wiki](https://wiki.hypr.land/Configuring/Start/)。

### 附加配置

Caelestia 是一个由 dotfiles（本仓库）、[shell][shell-repo] 和 [CLI][cli-repo] 组成的生态系统。
shell 的配置由 `~/.config/caelestia/shell.json` 管理，用于调整 shell 行为。
CLI 配置文件（`~/.config/caelestia/cli.json`）用于调整 CLI 的主题和特殊工作区处理。
shell 与 CLI 的配置说明见各自仓库。

## 默认键位

> [!TIP]
> 所有键位都可以通过覆盖 `hypr-vars.lua` 中对应的变量来自定义（shell 重启 / 关闭键位除外）。
> 详见[配置](#配置)。

### 启动器

| 快捷键 | 动作 |
| --- | --- |
| `Super`（按下并松开） | 打开启动器 |

---

### 工作区

| 快捷键 | 动作 |
| --- | --- |
| `Super + 1~9, 0` | 切换到工作区 1~10 |
| `Super + Alt + 1~9, 0` | 将窗口移到工作区 1~10 |
| `Ctrl + Super + 1~9, 0` | 切换到工作区组（×10） |
| `Ctrl + Super + Alt + 1~9, 0` | 将窗口移到工作区组 |
| `Super + Alt + S`、`Ctrl + Super + Shift + Up` | 将窗口移到特殊工作区 |
| `Ctrl + Super + Shift + Down` | 将窗口移出特殊工作区 |
| `Super + Alt + Scroll Down`、`Super + Alt + Page_Down`、`Ctrl + Super + Shift + Right` | 将窗口移到下一个工作区 |
| `Super + Alt + Scroll Up`、`Super + Alt + Page_Up`、`Ctrl + Super + Shift + Left` | 将窗口移到上一个工作区 |
| `Super + Scroll Down`、`Ctrl + Super + Right`、`Super + Page_Down` | 切换到下一个工作区 |
| `Super + Scroll Up`、`Ctrl + Super + Left`、`Super + Page_Up` | 切换到上一个工作区 |
| `Ctrl + Super + Scroll Down` | 切换到下一个工作区组 |
| `Ctrl + Super + Scroll Up` | 切换到上一个工作区组 |

---

### 窗口组

| 快捷键 | 动作 |
| --- | --- |
| `Alt + Tab` | 切换到组内下一个窗口 |
| `Shift + Alt + Tab` | 切换到组内上一个窗口 |
| `Ctrl + Alt + Tab` | 切换到下一个组 |
| `Ctrl + Shift + Alt + Tab` | 切换到上一个组 |
| `Super + U` | 将窗口移出组 |
| `Super + Comma` | 切换组 |
| `Super + Shift + Comma` | 锁定当前组 |

---

### 窗口操作

| 快捷键 | 动作 |
| --- | --- |
| `Super + Minus`、`Super + Alt + Left` | 将窗口向左分屏 |
| `Super + Equal`、`Super + Alt + Right` | 将窗口向右分屏 |
| `Super + Shift + Minus`、`Super + Alt + Up` | 将窗口向上分屏 |
| `Super + Shift + Equal`、`Super + Alt + Down` | 将窗口向下分屏 |
| `Super + Left/Right/Up/Down` | 聚焦指定方向的窗口 |
| `Super + Shift + Left/Right/Up/Down` | 将窗口移向指定方向 |
| `Super + LMB`、`Super + Z + LMB` | 移动窗口（拖拽） |
| `Super + RMB`、`Super + X + LMB` | 调整窗口大小（拖拽） |
| `Ctrl + Super + Backslash` | 将窗口居中 |
| `Ctrl + Super + Alt + Backslash` | 将窗口调整为屏幕的 55×70% 并居中 |
| `Super + Alt + Backslash` | 为窗口启用画中画模式 |
| `Super + P` | 切换窗口固定模式 |
| `Super + F` | 切换窗口全屏模式 |
| `Super + Alt + F` | 切换窗口最大化模式 |
| `Super + Alt + Space` | 切换窗口浮动模式 |
| `Super + Q` | 关闭窗口 |

---

### 特殊工作区开关

| 快捷键 | 动作 |
| --- | --- |
| `Super + S` | 切换特殊工作区 |
| `Ctrl + Shift + Escape` | 切换系统监视器工作区 |
| `Super + M` | 切换音乐工作区 |
| `Super + D` | 切换通讯工作区 |
| `Super + R` | 切换待办工作区 |

---

### 应用

| 快捷键 | 动作 |
| --- | --- |
| `Super + T` | 终端（默认：foot） |
| `Super + W` | 浏览器（默认：firefox） |
| `Super + C` | 编辑器（默认：codium） |
| `Super + E` | 文件管理器（默认：thunar） |
| `Ctrl + Alt + V` | 音频设置（默认：pwvucontrol） |

---

### 工具

| 快捷键 | 动作 |
| --- | --- |
| `Print` | 截图 |
| `Super + Shift + S` | 截图（冻结画面） |
| `Super + Shift + Alt + S` | 截图（区域） |
| `Ctrl + Alt + R` | 录制全屏 |
| `Super + Alt + R` | 录制声音 |
| `Super + Shift + Alt + R` | 录制区域 |
| `Super + Shift + C` | 取色器 |

---

### 媒体

| 快捷键 | 动作 |
| --- | --- |
| `Ctrl + Super + Space` | 播放 / 暂停 |
| `Ctrl + Super + Equal` | 下一首 |
| `Ctrl + Super + Minus` | 上一首 |
| `Ctrl + Super + Backspace` | 停止播放 |
| `Super + Shift + M` | 静音 |

---

### 杂项

| 快捷键 | 动作 |
| --- | --- |
| `Ctrl + Alt + Delete` | 打开 shell 会话菜单 |
| `Super + N` | 切换 shell 侧边栏 |
| `Ctrl + Alt + C` | 清除 shell 通知 |
| `Super + K` | 显示所有 shell 面板 |
| `Super + L` | 锁屏 |
| `Super + Alt + L` | 恢复 shell 锁屏 |
| `Super + Shift + L` | 执行睡眠命令 |

---

### 剪贴板 / Emoji

| 快捷键 | 动作 |
| --- | --- |
| `Super + V` | 打开剪贴板历史 |
| `Super + Alt + V` | 打开剪贴板历史（删除模式） |
| `Ctrl + Shift + Alt + V` | 粘贴最近的剪贴板条目 |
| `Super + Period` | 打开 Emoji 选择器 |

---

### Shell

| 快捷键 | 动作 |
| --- | --- |
| `Ctrl + Super + Alt + R` | 重启 shell |
| `Ctrl + Super + Shift + R` | 关闭 shell |

[shell-repo]: https://github.com/caelestia-dots/shell
[cli-repo]: https://github.com/caelestia-dots/cli
