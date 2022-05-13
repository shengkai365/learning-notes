## Tmux Cheat Sheet

**tmux 特点: **

- 分屏, 管理多个会话
- 断开Terminal，可继续运行进程

**tmux 安装**

```
$ sudo apt-get install tmux # Ubuntu或Debian
$ sudo yum install tmux # CentOs或Fedora
$ brew install tmux # MacOS
```

**tmux 结构**

`tmux------------->>>Sessions--------->>>Windows------------>>>Panes`

### 1. Sessions

| 功能                   | 命令                            |
| ---------------------- | ------------------------------- |
| 新建会话并命名 | `tmux new -s <session-name>`    |
| 挂起会话 | `ctrl + a + d`   ：  `tmux detach` |
| 查看所有会话 | `tmux ls`   ：    `tmux list-session`   ：    `Ctrl + b + s` |
| 连接已存在会话 | `tmux a`  ：   `tmux a -t <session-name>` |
| 删除会话 | `tmux kill-session -t <session-name>` |
| 切换会话 | `tmux switch -t < session-name>` |
| 重命名会话 | `Ctrl + b + $`   ：   `tmux rename-session -t <old> <new>` |



### 2. Windows(个人不常用)

| 功能             | 命令                                                         |
| ---------------- | ------------------------------------------------------------ |
| 创建新窗口       | `Ctrl + a + c` || `tmux new-window`                          |
| 切换到上一个窗口 | `Ctrl + a + p`                                               |
| 切换到下一个窗口 | `Ctrl + a + n`                                               |
| 切换到指定窗口   | `Ctrl + a + <name>` || `tmux select-window -t <window_name>` |
| 重命名窗口       | `Ctrl + a + ,` || `tmux rename-window <new_name>`            |
| 从列表中选择窗口 | `Ctrl + a + w`                                               |



### 3. Panes

| 功能                 | 命令                                      |
| -------------------- | ----------------------------------------- |
| 左右划分窗格         | `Ctrl + a + %`  || `tmux split-window -h` |
| 上下划分窗格         | `Ctrl + a + "` || `tmux split-window`     |
| 上下左右切换窗格     | `Ctrl + a + <arrow key>`                  |
| 关闭窗格             | `Ctrl + a + x` || `Ctrl + d`              |
| 将当前窗格(取消)全屏 | `Ctrl + a + z`                            |



### 4. Tips

- 复制到`tmux`剪切板

  (1)`Ctrl + a + [`  (2) 鼠标选中文本  (3)粘贴：`Ctrl + a + ]`

- 复制到系统剪切板

  (1)按住`Shift`   (2) 鼠标选中文本  (3) 复制: `Ctrl + insert` (4)粘贴: `Shift + insert`

- `tmux` 卡死的时候 : `Ctrl + xxxxx`

- 鼠标点击可以选择panes

- 鼠标拖动pane之间的分割线，可以调整分割线的位置。



### 5. 配置文件：`.tmux.conf`

```bash
set-option -g status-keys vi
setw -g mode-keys vi

setw -g monitor-activity on

# setw -g c0-change-trigger 10
# setw -g c0-change-interval 100

# setw -g c0-change-interval 50
# setw -g c0-change-trigger  75


set-window-option -g automatic-rename on
set-option -g set-titles on
set -g history-limit 100000

#set-window-option -g utf8 on


# set command prefix
set-option -g prefix C-a

unbind-key C-b
bind-key C-a send-prefix


bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R

bind -n M-Left select-pane -L
bind -n M-Right select-pane -R
bind -n M-Up select-pane -U
bind -n M-Down select-pane -D

bind < resize-pane -L 7
bind > resize-pane -R 7
bind - resize-pane -D 7
bind + resize-pane -U 7


bind-key -n M-l next-window
bind-key -n M-h previous-window



set -g status-interval 1
# status bar
set -g status-bg black
set -g status-fg blue


#set -g status-utf8 on
set -g status-justify centre
set -g status-bg default
set -g status-left " #[fg=green]#S@#H #[default]"
set -g status-left-length 20


# mouse support
# for tmux 2.1
# set -g mouse-utf8 on
set -g mouse on
#
# for previous version
#set -g mode-mouse on
#set -g mouse-resize-pane on
#set -g mouse-select-pane on
#set -g mouse-select-window on


#set -g status-right-length 25
set -g status-right "#[fg=green]%H:%M:%S #[fg=magenta]%a %m-%d #[default]"

# fix for tmux 1.9
bind '"' split-window -vc "#{pane_current_path}"
bind '%' split-window -hc "#{pane_current_path}"
bind 'c' new-window -c "#{pane_current_path}"

# run-shell "powerline-daemon -q"

# vim: ft=conf
```

