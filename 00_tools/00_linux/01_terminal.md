[TOC]

# screen

- 创建会话 screen -S <sessionName>
- 查看会话列表 screen -ls
- detach切换会话到后台 ctrl + a + d
- retach 重连screen -r <part of pid or title>
- execute 执行命令screen -X -S <pid> quit
- kill 关闭windows ctrl + a + k （关闭window而非session当仅有一个window时无差别）
- create 创建新窗口 ctrl + a + c
- 窗口切换 next: ctrl + a + n, previsou: ctrl + a + p
- 查看窗口列表 ctrl + a + w
- 退出所有窗口 ctrl + a + \
- 跳转指定window ctrl + a + <num>
- 查看所有窗口并跳转指定列表 ctrl + a + "
- rename重命名窗口 ctrl + a + A
- split into mutiple panes 分割 ctrl + a + |
- 切换panel ctrl + a + tab
- 水平切分 ctrl + a + S
- 关闭pane ctrl + a + X

# tmux

# vim

# emacs

