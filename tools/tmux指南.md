# tmux用法指南

## 1. 如何在tmux中启动鼠标滚路？

- 进入tmux后，`ctrl+b`后，按冒号，进入命令行模式；

- 输入以下命令：

  ```bash
  set -g mouse on
  ```

启动后可以在tmux窗口中使用滚轮，并用鼠标点击切换窗口。

