| helo | heal |
| ---- | ---- |
|      |      |

# 光标移动

1. 移动到单词的最前面：option + ←
2. 移动到单词最末尾：option + →
3. 将当前行代码移动到上一行：option + ↑
4. 将当前行代码移动到下一行：option + ↓
5. 移动到当前行最前面：cmd + ←
6. 移动到当前行最末尾：cmd + →
7. 花括号之间跳转：cmd + shift +
8. 移动到文档第一行或最后一行：cmd + ↑ / cmd + ↓

# 文本选择

基于单词，行，文档的光标操作加上个shift键，就可以移动光标的同时选择文本；例如，选择当前光标所在位置到当前行最前面的代码：cmd + ← + shift

## 添加注释

1. 注释一行代码：cmd + /
2. 注释一整段代码：option + shift + A

## 格式化代码

1. 格式化代码：option + shift + F
2. 格式化选中行代码：cmd + K cmd + F
3. 代码缩进：cmd + shift + P

# 文件、符号、代码之间的快速跳转

1. control+ tab(同时按住)，继续按着control键，松开tab键： 打开当前打开文件的列表，选择要打开文件，松开control就能打开对应文件
2. cmd + P打开最近打开文件列表，同时列表顶部出现搜索框，搜索文件名，回车（enter），可以再当前窗口打开对应文件；使用cmd + enter会在新的编辑器窗口打开这个文件
3. control + G：行跳转，输入对应数字回车，可以跳转到当前文件的当前行
4. cmd + P(输入文件名 + “:” + 行数)：跳转到指定文件的指定行数
5. cmd + shift + O：调出当前文件的符号（函数名等），使用方向键或者搜索，回车，就能跳转到你想要的符号；如果输入“:”可以对当前文件的所有符号进行分类
6. cmd + T：打开多个文件，搜索多个文件中的符号
7. F12：跳转到函数的定义处
8. cmd + F12：跳转到函数的实现位置；注：js中没有接口的概念，定义和实现是相同的，所以js中的F12和Cmd + F12效果是一样的
9. shift + F12：打开函数引用的预览（把光标放在函数或者类上，按shift+F12可以打开一个引用列表和内嵌编辑器）

## 全局

1. Command + Shift + P / F1 显示命令面板
2. Command + P 快速打开
3. Command + Shift + N 打开新窗口
4. Command + W 关闭窗口

## 基本

1. Option + Up 向上移动行
2. Option + Down 向下移动行
3. Option + Shift + Up 向上复制行
4. Option + Shift + Down 向下复制行



- Command + Shift + K 删除行
- Command + Enter 下一行插入
- Command + Shift + Enter 上一行插入
- Command + Shift + \ 跳转到匹配的括号
- Command + [ 减少缩进
- Command + ] 增加缩进
- Home 跳转至行首
- End 跳转到行尾
- Command + Up 跳转至文件开头
- Command + Down 跳转至文件结尾
- Ctrl + PgUp 按行向上滚动
- Ctrl + PgDown 按行向下滚动
- Command + PgUp 按屏向上滚动
- Command + PgDown 按屏向下滚动
- Command + Shift + [ 折叠代码块
- Command + Shift + ] 展开代码块
- Command + K Command + [ 折叠全部子代码块
- Command + K Command + ] 展开全部子代码块
- Command + K Command + 0 折叠全部代码块
- Command + K Command + J 展开全部代码块
- Command + K Command + C 添加行注释
- Command + K Command + U 移除行注释
- Command + / 添加、移除行注释
- Option + Shift + A 添加、移除块注释
- Option + Z 自动换行、取消自动换行

## 多光标与选择

- Option + 点击 插入多个光标
- Command + Option + Up 向上插入光标
- Command + Option + Down 向下插入光标
- Command + U 撤销上一个光标操作
- Option + Shift + I 在所选行的行尾插入光标
- Command + I 选中当前行
- Command + Shift + L 选中所有与当前选中内容相同部分
- Command + F2 选中所有与当前选中单词相同的单词
- Command + Ctrl + Shift + Left 折叠选中
- Command + Ctrl + Shift + Right 展开选中
- Alt + Shift + 拖动鼠标 选中代码块
- Command + Shift + Option + Up 列选择 向上
- Command + Shift + Option + Down 列选择 向下
- Command + Shift + Option + Left 列选择 向左
- Command + Shift + Option + Right 列选择 向右
- Command + Shift + Option + PgUp 列选择 向上翻页
- Command + Shift + Option + PgDown 列选择 向下翻页

## 查找替换

- Command + F 查找
- Command + Option + F 替换
- Command + G 查找下一个
- Command + Shift + G 查找上一个
- Option + Enter 选中所有匹配项
- Command + D 向下选中相同内容
- Command + K Command + D 移除前一个向下选中相同内容

## 导航

- Command + T 显示所有符号
- Ctrl + G 跳转至某行
- Command + P 跳转到某个文件
- Command + Shift + O 跳转到某个符号
- Command + Shift + M 打开问题面板
- F8 下一个错误或警告位置
- Shift + F8 上一个错误或警告位置
- Ctrl + Shift + Tab 编辑器历史记录
- Ctrl + - 后退
- Ctrl + Shift + - 前进
- Ctrl + Shift + M Tab 切换焦点

## 编辑器管理

- Command + W 关闭编辑器
- Command + K F 关闭文件夹
- Command + \ 编辑器分屏
- Command + 1 切换到第一分组
- Command + 2 切换到第二分组
- Command + 3 切换到第三分组
- Command + K Command + Left 切换到上一分组
- Command + K Command + Right 切换到下一分组
- Command + K Command + Shift + Left 左移编辑器
- Command + K Command + Shift + Right 右移编辑器
- Command + K Left 激活左侧编辑组
- Command + K Right 激活右侧编辑组

## 文件管理

- Command + N 新建文件
- Command + O 打开文件
- Command + S 保存文件
- Command + Shift + S 另存为
- Command + Option + S 全部保存
- Command + W 关闭
- Command + K Command + W 全部关闭
- Command + Shift + T 重新打开被关闭的编辑器
- Command + K Enter 保持打开
- Ctrl + Tab 打开下一个
- Ctrl + Shift + Tab 打开上一个
- Command + K P 复制当前文件路径
- Command + K R 在资源管理器中查看当前文件
- Command + K O 新窗口打开当前文件

## 显示

- Command + Ctrl + F 全屏、退出全屏
- Command + Option + 1 切换编辑器分屏方式（横、竖）
- Command + + 放大
- Command + - 缩小
- Command + B 显示、隐藏侧边栏
- Command + Shift + E 显示资源管理器 或 切换焦点
- Command + Shift + F 显示搜索框
- Ctrl + Shift + G 显示Git面板
- Command + Shift + D 显示调试面板
- Command + Shift + X 显示插件面板
- Command + Shift + H 全局搜索替换
- Command + Shift + J 显示、隐藏高级搜索
- Command + Shift + C 打开新终端
- Command + Shift + U 显示输出面板
- Command + Shift + V Markdown预览窗口
- Command + K V 分屏显示 Markdown预览窗口

## 调试

- F9 设置 或 取消断点
- F5 开始 或 继续
- F11 进入
- Shift + F11 跳出
- F10 跳过
- Command + K Command + I 显示悬停信息

## 集成终端

- Ctrl + `显示终端 Ctrl + Shift +` 新建终端
- Command + Up 向上滚动
- Command + Down 向下滚动
- PgUp 向上翻页
- PgDown 向下翻页
- Command + Home 滚动到顶部
- Command + End 滚动到底部
