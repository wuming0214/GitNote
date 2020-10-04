### GitHub 介绍
	GitHub 是一个面向开源及私有软件项目的托管平台，因为只支持 Git 作为唯一的版本库格式进行托管，故名 GitHub。

### 注册 GitHub
	1. 准备邮箱
	  最好不用 163 邮箱，有可能会收不到 GitHub 发来的邮件。

	2. 注册 GitHub 账号
	  (1) 进入注册页面：https://github.com
	  (2) 输入注册信息后，点击 Sign up for GitHub
	    Username：不能被占用
	    Email：不能被占用
	    Password：不能太简单
	  (3) 核实账户：回答问题，证明你是人类
	  (4) 加入免费计划
	  (5) 回答问卷，完成个性化设置
	  (6) 核实 Email 地址

	注册成功！并已经处于登录状态！
	
### 修改 GitHub 账号头像
	首页右上角头像 → Settings → Edit profile picture → Upload a photo... → Set new profile picture
	
### 创建远程库
	1. 登录 GitHub
	2. 首页右上角＋号 → New repository
	3. 填写 Repository name 和 Description
	4. 选择仓库类型 Public 或 Private
	5. 点击 Create Repository
	6. 新建的远程仓库的主页是 https://github.com/${username}/${repositoryName}

### 组建团队	
	1. 邀请合作者访问个人仓库
	  (1) 邀请人登录 GitHub
	  (2) 进入仓库主页，网址形如 https://github.com/${username}/${repositoryName}
	  (3) Settings → Manage access → Invite a collarotor
	    a. 在仓库主页点击 Settings 进入仓库设置页面
	    b. 在仓库设置页面的左侧边栏点击 Manage access
	    d. 点击 Invite a collaborator，输入用户名或邮箱，根据提示完成邀请。
	  (4) 被邀请人将会收到一封邀请邮件
		
	2. 被邀请人接受合作邀请
	  (1) 被邀请人登录 GitHub
	  (2) 点击邀请邮件中的 View invitation 链接，进入邀请页面
	  (3) 在邀请页面选择接受或拒绝邀请
	  (4) 接受邀请后，被邀请人具有对该仓库的合作者权限，可以向该仓库 push 本地修改。

### 配置 SSH 免密登录 GitHub
	GitHub 上的远程库有 HTTPS 和 SSH 两种形式的地址，使用 SSH 形式的地址在 push 操作的时候可以免密登录。
	配置方法如下：

	1. 生成 SSH Key
	  (1) 打开 Git Bash
	  (2) ssh-keygen -t rsa -C githubEmail@example.com
	    然后一路回车，使用默认值即可。
	  注：
	     生成的私钥保存在 ~/.ssh/id_rsa 文件
	     生成的公钥保存在 ~/.ssh/id_rsa.pub 文件
	
	2. 将公钥关联到 GitHub 账号
	  (1) 首页右上角头像 → Settings → SSH and GPG keys → New SSH key
	  (2) 输入 Title、Key
	    Title: 随便写，比如 myKey
	    Key: 将公钥复制到这个地方
	  (3) 点击 Add SSH Key

### 使用 GitHub 跨团队协作
	❄ 概念：
	  • fork：服务端的代码克隆
	  • pull request：pull request 是一个 request，它的目的是让别人 pull 你的东西。

	❄ 流程：
	  ☛ PR 发起者操作：
	    1. fork 远程库
	      仓库主页 → fork
	
	    2. 修改 fork 出来的远程库
	      (1) 克隆到本地
	      (2) 本地修改
	      (3) 推送到远程
	
	    3. 发起 pull request
	      (1) 进入 fork 出来的仓库主页
	      (2) 点击 Pull requests 
	      (3) 点击 New pull request
	        a. 下拉选择 Base repository，Base ref，Head repository，Head ref，点击 Create pull request。
	        b. 填写 pull request 标题、描述，点击 Create pull request。
	
	  ☛ PR 接受者操作：
	    1. 查看仓库的 pull request 列表
	      (1) 进入仓库主页
	      (2) 点击 Pull requests
	
	    2. 进入指定的 pull request 主页
	
	    3. 审核代码
	      (1) 在 pull request 的 Conversations 页与 PR 发起者对话
	      (2) 在 pull request 的 Commits 页查看 PR 发起者的提交
	      (3) 在 pull request 的 Files changed 页查看 PR 发起者修改的文件
	
	    4. 接受 pull request
	      (1) 在 pull request 的 Conversations 页面点击 Merge pull request
	      (2) 填写本次操作的日志信息
	      (3) 点击 Confirm merge 确认合并