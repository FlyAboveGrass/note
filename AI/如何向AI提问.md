
### 撰写清楚的指令

1. 确保你的提问中包含了所有重要的细节和背景信息
2. 让系统扮演一个角色
3. 利用分隔符来区分输入的不同部分
4. 明确说明任务所需要的每个步骤。
5. 明确输出的长度要求。（按照指定的段落数或者要点数来构建输出）

### 提供参考文本
1. 使用参考的文本来构建你的答案
2. 指导模型用引用的文本来回答问题。（输出中的引用可以通过在所提供的文本中匹配字符串来进行验证 ）

### 复杂任务拆分成简单的任务

1. 对查询的内容进行分类，然后为这些类别的的任务指定相关的指令，将一个任务拆解成多个阶段，每一个阶段的查询只包含下一个阶段的结果。（这样可以降低错误率，但是会增大成本，因为较长的提示词需要更多的编写时间）
2.  针对需要长时间对话的程序，应概括或者过滤之前的对话内容。
	1. 历史的 token 太长，会导致计算量过大，回答的精度降低
3. 逐段归纳长文档并递归地构建完整的摘要。
	1. 因为模型的上下文长度是固定的，他们无法一次性的总结超过上下文的长度减去所生成摘要长度的文本

### 给模型更多的时间去思考

1. 在让模型直接得出结论之前，指导模型得出自己的答案。
	1. 例如，不要直接询问模型一个问题是否正确，而是先向他提问，让他得到结果之后与自己的结果进行对比，再让他回答是否正确。
	2. 在直接回答我们的问题之前，要求模型对他的回答进行解释。
2. 询问模型是否有遗漏的地方。

### 运用外部工具

1. 使用基于嵌入的搜索来实现高效的知识检索。
	1. 在给模型提问的时候，可以给他提供对应的语块相近的内容，这会让他的向量查询更加准确和快速。例如在问他关于某部电影的看法的时候，提供电影的导演、演员等。
2. 使用代码来进行更准确的计算
	1. 一些复杂的操作，模型可能会出错。可以让他编写代码来实现，用代码执行结果代替模型的直接回答。
3. 

### 系统的对变更进行测试

1. 参考标准答案来评估模型的准确性。