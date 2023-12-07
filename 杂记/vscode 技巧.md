
### 自定义任务

> **Tip:** To get access to the global scope `tasks.json` file, open the Command Palette (⇧⌘P) and run the **Tasks: Open User Tasks** command.

[Tasks in Visual Studio Code](https://code.visualstudio.com/docs/editor/tasks#_task-autodetection)


[oh-my-zsh 中 git branch 乱码显示问号问题分析和临时解决方案 - 简书](https://www.jianshu.com/p/af56b1ad9ea0)
homebrew 安装的 git 版本不是最新
	解决方案： 更换 homebrew 镜像源

- `git pull origin develop: develop` 如果本地与 develop 的记录不一致，在合并 develop 到 develop 后，还会将 develop 合并到本地分支
- `git fetch . sourceBranch:develop` develop 分支上的合并记录会丢失，如
	- commit 1 - commit 2 - mergeCommit 只会拉取到 commit 1 - commit 2



`git branch --show-current` 和 `git rev-parse --abbrev-ref HEAD` 命令都可以用于获取当前所在的 Git 分支名称。但是，它们之间有一些不同点：

- `git branch --show-current` 命令是 Git 2.22 版本中新增的，可以直接显示当前所在分支的名称，而不需要进行额外的选项或管道。如果版本较旧，则执行该命令将会报错。
- `git rev-parse --abbrev-ref HEAD` 命令可以用于获取 HEAD 引用的简短引用名称，即当前所在分支名称。如果 HEAD 在一个分支上，则该命令将输出分支名称；如果 HEAD 在一个提交 ID 上，则该命令将输出 HEAD 引用的全局唯一对象名称（即 commit ID）。

优先考虑使用 `git branch --show-current` 命令


`git pull origin develop: develop` 如果本地与 develop 的记录不一致，在合并 develop 到 develop 后，还会将 develop 合并到本地分支
- `git fetch . sourceBranch:develop` develop 分支上的合并记录会丢失，如
	- commit 1 - commit 2 - mergeCommit 只会拉取到 commit 1 - commit 2



