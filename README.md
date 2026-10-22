# kitty 配置

本仓库的 [kitty.conf](./kitty.conf) 适用版本为 **kitty 0.49.2**，使用 TokyoNight Night 配色，并配置了窗口、标签页和分屏等常用快捷键。

## 配置特点

- 配色：深色背景 `#1a1b26`，前景色 `#c0caf5`，背景不透明度 `0.98`。
- 字体：`JetBrainsMono Nerd Font Mono`，字号 `13.0`，字符单元高度 `105%`。
- 窗口：内边距 `1`，记住窗口大小。
- 标签页：位于顶部，至少有两个标签页时才显示标签栏。
- 布局：启用 `splits`（分屏）和 `stack`（仅显示当前分屏）。
- 光标：不闪烁的块状光标；输入文字时隐藏鼠标指针。
- 新建窗口、标签页和分屏时，使用当前终端的工作目录。

## 使用方式

### 1. 准备环境

先安装 kitty，以及配置中使用的 `JetBrainsMono Nerd Font Mono` 字体。可以检查当前 kitty 版本：

```bash
kitty --version
```

如果使用其他字体，请将 `kitty.conf` 中的 `font_family` 改为本机已安装的字体名称。kitty 的安装方式见[官方安装文档](https://sw.kovidgoyal.net/kitty/binary/)。

### 2. 临时加载配置

在本仓库根目录执行：

```bash
kitty --config "$PWD/kitty.conf"
```

这会打开使用本仓库配置的新 kitty 窗口，不修改默认配置文件。也可以在任意目录使用绝对路径：

```bash
kitty --config /path/to/kitty_config/kitty.conf
```

将 `/path/to/kitty_config` 替换为实际仓库路径。临时试用期间，可以直接编辑仓库中的 `kitty.conf`，再执行上述命令打开新窗口查看效果。

### 3. 设为默认配置

通常的配置路径是 `~/.config/kitty/kitty.conf`。如果设置了 `XDG_CONFIG_HOME`，则使用其下的 `kitty/kitty.conf`；如果设置了 `KITTY_CONFIG_DIRECTORY`，则优先使用该目录下的 `kitty.conf`。

推荐建立软链接，之后修改仓库文件即可更新默认配置。在本仓库根目录执行以下 Bash 命令，已有配置会先备份：

```bash
kitty_config_dir="${KITTY_CONFIG_DIRECTORY:-${XDG_CONFIG_HOME:-$HOME/.config}/kitty}"
mkdir -p "$kitty_config_dir"

if [ -e "$kitty_config_dir/kitty.conf" ] || [ -L "$kitty_config_dir/kitty.conf" ]; then
    mv "$kitty_config_dir/kitty.conf" \
       "$kitty_config_dir/kitty.conf.bak.$(date +%Y%m%d%H%M%S)"
fi

ln -s "$PWD/kitty.conf" "$kitty_config_dir/kitty.conf"
```

完成后正常启动 kitty 即可。软链接依赖仓库路径，移动仓库后需要重新建立链接。

如果希望独立保存配置，也可以在上述命令中将最后一行替换为：

```bash
cp "$PWD/kitty.conf" "$kitty_config_dir/kitty.conf"
```

复制方式不会自动同步仓库的后续修改。

### 4. 编辑和重新加载

使用默认配置时：

- `Ctrl+,`：打开配置文件进行编辑。
- `Ctrl+Shift+,`：保存修改后，重新加载配置。

部分设置需要重启 kitty 才能生效。配置加载和设置说明可查阅 [kitty.conf 官方文档](https://sw.kovidgoyal.net/kitty/conf/)。

## 快捷键说明

以下列出本仓库 `kitty.conf` 中显式配置的快捷键。同一行中的多个组合键执行相同功能；`+` 表示同时按下，`,` 表示逗号键，`↑`、`↓`、`←`、`→` 表示方向键。

本配置没有清空 kitty 的默认快捷键，未被覆盖的默认绑定仍然保留。

### 原生窗口与退出

这里的“原生窗口”指桌面上的独立 kitty 窗口；每个原生窗口可以包含多个标签页，每个标签页可以包含多个分屏。

- `Ctrl+Shift+N`：新建原生窗口，继承当前工作目录。
- `Ctrl+W`：关闭当前原生窗口，包括其中的所有标签页和分屏。
- `Ctrl+Enter`：切换全屏。
- `Ctrl+Shift+Q`：退出当前 kitty 进程，关闭该进程的所有原生窗口。

**注意：配置设置了 `confirm_os_window_close 0`，关闭窗口或退出时不显示确认提示。`Ctrl+W` 会关闭整个原生窗口；只想关闭当前分屏时，请使用 `Ctrl+Shift+W`。**

### 标签页

- `Ctrl+Shift+T`：在当前标签页后新建标签页，继承当前工作目录。
- `Ctrl+Tab` / `Ctrl+PageDown`：切换到下一个标签页。
- `Ctrl+Shift+Tab` / `Ctrl+PageUp`：切换到上一个标签页。
- `Alt+1` 至 `Alt+9`：切换到第 1 至第 9 个标签页，按从左到右的顺序计数。
- `Alt+0`：切换到最右侧标签页。
- `Ctrl+Shift+R`：设置当前标签页标题。

当指定的标签页编号超出现有数量时，会切换到最右侧标签页。`Alt+0` 使用一个很大的正索引实现这一行为，不表示“切换到第 10 个标签页”。

### 创建、关闭和放大分屏

- `Ctrl+Shift+O`：左右分屏，在当前分屏右侧新建终端。
- `Ctrl+Shift+E`：上下分屏，在当前分屏下方新建终端。
- `Ctrl+Shift+W`：关闭当前分屏；关闭标签页中的最后一个分屏时，该标签页也会关闭。
- `Ctrl+Shift+Enter`：在 `stack` 与之前的布局之间切换，用于放大当前分屏，再按一次恢复。

新建分屏会继承当前工作目录，并先切回 `splits` 布局，因此放大分屏后也可以直接继续分屏。分屏创建位置的含义见 [launch 官方文档](https://sw.kovidgoyal.net/kitty/launch/#cmdoption-launch-location)。

### 切换分屏焦点

- `Alt+←` / `Ctrl+Alt+←`：切换到左侧相邻分屏。
- `Alt+→` / `Ctrl+Alt+→`：切换到右侧相邻分屏。
- `Alt+↑` / `Ctrl+Alt+↑`：切换到上方相邻分屏。
- `Alt+↓` / `Ctrl+Alt+↓`：切换到下方相邻分屏。

### 调整分屏大小

- `Ctrl+Shift+←`：缩窄当前分屏，步长为 2 个字符单元。
- `Ctrl+Shift+→`：加宽当前分屏，步长为 2 个字符单元。
- `Ctrl+Shift+↑`：增加当前分屏高度，步长为 2 个字符单元。
- `Ctrl+Shift+↓`：减小当前分屏高度，步长为 2 个字符单元。

实际调整受分屏布局和可用空间限制；这里的步长是字符单元，不是像素。

### 复制与粘贴

- `Ctrl+Shift+C` / `Ctrl+Insert`：复制选中的文本到系统剪贴板。
- `Ctrl+Shift+V`：粘贴系统剪贴板内容。
- `Shift+Insert`：粘贴主选择区（primary selection）内容，通常是鼠标选中的文本；它与普通系统剪贴板是不同的选择区。

### 字号

- `Ctrl+=` / `Ctrl++`：字号增加 `1.0`。
- `Ctrl+-`：字号减小 `1.0`。
- `Ctrl+0`：恢复配置中的默认字号，当前为 `13.0`。

其中 `Ctrl++` 表示 Ctrl 加上加号键。这些操作作用于当前原生窗口，包含该窗口内的标签页和分屏，不会写回配置文件。

### 滚动与命令提示符跳转

- `Shift+PageUp`：向上滚动一页。
- `Shift+PageDown`：向下滚动一页。
- `Shift+Home`：滚动到已保留历史内容的顶部。
- `Shift+End`：滚动到底部。
- `Ctrl+Shift+PageUp`：跳转到上一个命令提示符。
- `Ctrl+Shift+PageDown`：跳转到下一个命令提示符。

命令提示符跳转需要 shell integration 正常工作。本配置的 `shell_integration no-cursor` 只禁止 shell integration 改变光标形状，其余集成功能仍然启用。详情见[官方 shell integration 设置说明](https://sw.kovidgoyal.net/kitty/conf/#opt-kitty.shell_integration)。

### 配置与命令面板

- `Ctrl+Shift+P`：打开命令面板，搜索和执行 kitty 操作。
- `Ctrl+,`：编辑配置文件。
- `Ctrl+Shift+,`：重新加载配置文件。

需要扩展快捷键时，可参考 [kitty 可绑定动作列表](https://sw.kovidgoyal.net/kitty/actions/)。
