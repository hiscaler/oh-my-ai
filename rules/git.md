# Git 使用规则

使用 /git-message 命令的情况下忽略本规则

## Git Message 提交格式

提交格式为 `Sign: Message`，提交内容过长的情况下采用结构化书写（单行摘要+空行+详细描述）

Sign 有以下几种方式：

- New: 添加了新功能
- Chg: 修改了某个已经存在的功能
- Enh: 调整或者重构了某个原本已经存在的功能
- Bug: 修复了原有代码的问题
- Doc: 文档修改

示例：New: 支持 Excel 读取

## Git 提交流程

Git 处理只针对 Staged 中的文件，如果 Staged 中没有暂存文件，直接跳过，并提示用户

- 仅提交 Staged 中的文件
- 如果发现文件用户修改过了，以用户的修改结果为准
- 提交前应该**格式化修改过的代码**，当前未修改的代码不需要格式化
- 读取并分析 staged 暂存文件修改内容，按照 Git Message 提交格式撰写提交内容，撰写完毕后提示用户确认，待用户确认完毕后再提交

## Git 指令

- 撰写/修改 Swagger: #swagger 文件路径
- 格式化：#format
- 撰写 Git Message: #gm, #gitmessage, #write, #wgm
- 提交 Git: #commit, #submit

请严格按照以上指令执行操作，没有接收到以上指令或者指令不完整均不要执行任何操作，指令不完整的情况下需提示用户

### GIT 联合指令

多个指令组合方式，不同指令使用 | 分隔，比如 #gm|#commit 表示撰写 Git Message 并提交到 Git，多个指令按照先后顺序依次执行