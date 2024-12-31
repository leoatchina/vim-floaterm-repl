# vim-floaterm-repl

这是一个基于 [vim-floaterm](https://github.com/voldikss/vim-floaterm) 的 Vim 插件，用于在浮动终端中与不同语言的 REPL (Read-Eval-Print Loop) 程序交互。

## 原理

该插件的主要功能是允许用户将 Vim 编辑器中的代码片段发送到已启动的 REPL 程序中执行，并将结果显示在浮动终端中。其核心原理包括：

1. **REPL 程序管理:**  通过全局变量 `g:floaterm_repl_programs` 存储不同文件类型关联的 REPL 启动命令。例如，Python 文件类型关联 `ipython` 或 `python3` 命令。
2. **代码块标记:**  允许用户在代码中使用特定标记（例如 Python 中的 `# %%`）来定义代码块。插件会识别这些标记，方便用户按块发送代码。
3. **清屏和退出命令:**  为不同编程语言的 REPL 定义了清屏和退出的命令，例如 JavaScript 的 `.clear` 和 `.exit`。
4. **浮动终端交互:**  依赖 `vim-floaterm` 插件来创建和管理浮动终端，并在其中启动 REPL 程序。插件通过 `vim-floaterm` 提供的接口发送代码和命令。
5. **代码发送流程:**  当用户执行发送代码的命令时，插件会获取选定的代码范围（当前行、选定行、代码块等），然后将其发送到与当前文件类型关联的 REPL 进程。

## 命令

以下是 `vim-floaterm-repl` 插件提供的主要命令及其功能：

* **`FloatermReplStart`**: 启动当前文件类型的 REPL 程序。如果已配置多个可执行的 REPL 程序，会弹出一个列表供用户选择。
* **`FloatermReplSend [range]`**: 将指定范围的代码发送到 REPL。
    * 可以使用行号范围，例如 `:10,20FloatermReplSend` 发送第 10 到 20 行。
    * 如果没有指定范围，则发送当前行。
* **`FloatermReplSendVisual`**: 将可视模式下选中的代码发送到 REPL。
* **`FloatermReplSendBlock`**: 发送当前代码块到 REPL。代码块由 `g:floaterm_repl_block_mark` 定义的标记分隔。默认的标记是 `# %%`。
* **`FloatermReplSendFromBegin`**: 从文件开头发送到当前行（或上一行代码块标记）。
* **`FloatermReplSendToEnd`**: 从当前行发送到文件末尾。
* **`FloatermReplSendAll`**: 发送整个文件内容到 REPL。
* **`FloatermReplSendNewlineOrStart`**:
    * 如果 REPL 未启动，则启动 REPL。
    * 如果 REPL 已启动，则发送一个换行符到 REPL。
* **`FloatermReplSendClear`**: 发送清屏命令到 REPL，清除 REPL 终端的显示。
* **`FloatermReplSendExit`**: 发送退出命令到 REPL，关闭 REPL 进程。
* **`FloatermReplSendWord`**: 发送光标下的单词到 REPL。
* **`FloatermReplMark [range]`**: 标记一个代码块或可视选择，方便后续发送。
* **`FloatermReplSendMark`**: 发送之前使用 `FloatermReplMark` 命令标记的代码。
* **`FloatermReplQuickuiMark`**: （可能依赖 `vim-quickui` 插件）快速查看标记的代码。

## 配置

可以通过配置 Vim 全局变量来自定义插件的行为：

* **`g:floaterm_repl_programs`**:  一个字典，用于配置不同文件类型关联的 REPL 启动命令。
* **`g:floaterm_repl_block_mark`**: 一个字典，用于配置不同文件类型的代码块标记。
* **`g:floaterm_repl_clear`**: 一个字典，用于配置不同文件类型的 REPL 清屏命令。
* **`g:floaterm_repl_exit`**: 一个字典，用于配置不同文件类型的 REPL 退出命令。

**注意:** 使用此插件需要先安装 [vim-floaterm](https://github.com/voldikss/vim-floaterm) 插件。