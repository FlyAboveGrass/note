
# opencode

### 初始化自定义服务商（Provider）

先**不要直接启动** OpenCode，而是在终端执行以下命令：

`opencode auth login`

- 在服务商列表中，选择 **`other`**（在最下面，可以直接搜索）。
- 系统会提示你输入 **Provider ID**：  
	- 请填写一个**唯一标识名**（例如 `custom-hajimi`），后续配置需与之严格一致。
- 接着输入 **API Key**：  
     可填写任意内容（如 `dummy`），因为实际密钥可通过配置文件安全引用（见下一步）。

>  这一步的作用是让 OpenCode 在本地凭证管理器中注册一个自定义服务商，便于后续引用密钥。

### 配置中转站 API 地址

配置方式： [OpenCode 自定义服务商（中转站）接入指南 - 开发调优 - LINUX DO](https://linux.do/t/topic/1329050/34)

> 注意： 1.any router 不能用。 2. 模型一定要配置！

配置方式： 
![[Pasted image 20260212155758.png]]
![[Pasted image 20260212155919.png]]

# claude

配置方式：
![[Pasted image 20260212160347.png]]