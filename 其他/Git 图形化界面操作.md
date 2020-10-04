### 本文介绍
本文讲述在 Eclipse 里进行开发的同时，如何基于 Git 进行版本控制。
	
### 环境准备
Eclipse 从很早的版本开始就内置了 Git 插件，不需要额外安装。
	
### 设置全局忽略文件
	1. Eclipse 特定文件：Eclipse 为了管理我们创建的项目而维护的文件。
	  包括：
	    .classpath 文件
	    .project 文件
	    .settings 目录下所有文件
	
	2. 为什么要忽略 Eclipse 特定文件？
	  同一个团队中很难保证大家使用相同的 IDE。而 IDE 不同，相关项目的特定文件就可能不同。如果这些文件加入版本控制，那么开发时很可能需要为了这些文件解决冲突。
	
	3. 做法
	  (1) 编写忽略文件
	    vim ~/Java.gitigonre
	    # Compiled class file
	    *.class

	    # Log file
	    *.log
	
	    # BlueJ files
	    *.ctxt
	
	    # Mobile Tools for Java (J2ME)
	    .mtj.tmp/
	
	    # Package Files #
	    *.jar
	    *.war
	    *.nar
	    *.ear
	    *.zip
	    *.tar.gz
	    *.rar
	
	    # virtual machine crash logs, see http://www.java.com/en/download/help/error_hotspot.xml
	    hs_err_pid*
	
	    .classpath
	    .project
	    .settings
	    target
	
	    注：
	      可以参考 Github 提供的 .gitignore 模板集合：https://github.com/github/gitignore，里面有用于 Java 的 gitignore 模版。
	
	  (2) 设置全局忽略文件
	    a. Eclispe 菜单栏 → Window → Preferences → Team → Git → Configuration → User Settings
		b. 点击 Add Entry... 进入 Add a configuration entry 窗口，填写 Key, Value 设置全局忽略文件，点击 Add
	      Key: core.excludesfile
	      Value: ~/Java.gitignore
	    c. 点击 Apply
	 
### 本地库操作
	❄ 本地库初始化
	  方法一：
	    1. 项目 → 右键菜单 → Team → Share Project... → Git
	    2. 勾选 Use or create repository in parent folder of project
	    3. 在下面的列表中选中项目，点击 Create Repository
	    4. 点击 Finish

	  方法二：
	    1. $ mkdir 项目目录
	    2. $ cd 项目目录
	    3. $ git init
	    4. 在 Eclipse 中创建项目时指定项目位置为上述项目目录即可
	    注：
	      此方法需要单独安装 Git！
	
	❄ 设置仓库级别的签名
	  1. Eclispe 菜单栏 → Window → Preferences → Team → Git → Configuration → Repository Settings
	  
	  2. 选择 Repository 为指定仓库
	
	  3. 点击 Add Entry... 进入 Add a configuration entry 窗口，填写 Key, Value 设置用户名，点击 Add
	    Key: user.name
	    Value: eclipse_user
	
	  4. 点击 Add Entry... 进入 Add a configuration entry 窗口，填写 Key, Value 设置邮箱，点击 Add
	    Key: user.email
	    Value: eclipse_user@guigu.com
	
	  5. 点击 Apply
	
	❄ 工作区 → 暂存区
	  项目 → 右键菜单 → Team → Add to Index
	
	❄ 提交
	  1. 打开提交窗口
	    项目 → 右键菜单 → Team → Commit...
	  2. 输入提交说明
	  3. 点击 Commit

	❄ 查看状态
	  在 Eclipse 中可以通过观察文件图标中的装饰来判断文件的状态。
	  在 Eclipse 菜单栏 → Window → Preferences → Team → Git → Label Decorations 中可以查看装饰与文件状态的对应关系。
	  
### 远程库操作
	❄ 推送到远程库
	  1. 打开推送窗口
	    项目 → 右键菜单 → Team → Remote → Push...
	  2. 填写 URI、User、Password，点击 Next
	  3. 填写 Source ref、Destination ref，点击 Add Spec
	  4. 点击 Finish
	
	❄ 克隆远程库到本地，并作为 Maven 项目导入 Eclipse 中
	  远程仓库：
	    TestGit
	    │── .git
	    │── src
	    │   ├── main
	    │   │   ├── java
	    │   │   ├── resources
	    │   │   └── webapp
	    │   └── test
	    │       ├── java
	    │       └── resources
	    └── pom.xml

	  方法一：
	    1. 克隆远程库到本地
	      (1) 打开 Import Projects from Git 窗口
	        a. 打开导入窗口: Eclipse 菜单栏 → File → Import...
	        b. 选择导入向导: Projects from Git，点击 Next
	      (2) 选择 Clone URI，点击 Next
	      (3) 填写 URI、User、Password，点击 Next
	      (4) 选择 Select All 克隆所有分支，点击 Next
	      (5) 填写 Destination Directory 为 ${Eclipse 工作区目录}/${项目名称}，点击 Next
	
	    2. 导入项目
	      (1) 选择导入向导：Import as general project，点击 Next
	      (2) 输入项目名称，点击 Finish
	
	    3. 转换项目 General Project → Maven Project
	      项目 → 右键菜单 → Configure → Convert to Maven Project

	  方法二：
	    1. 克隆远程库到本地
	      $ git clone https://github.com/wuming0214/TestGit.git
	    2. 导入 Maven 项目
	      (1) 打开 Import Maven Projects 窗口
	        a. 打开导入窗口: Eclipse 菜单栏 → File → Import...
	        b. 选择导入向导: Existing Maven Projects，点击 Next
	      (2) 点击 Browse... 在文件管理器中选择 Maven 项目的根目录
	      (3) 点击 Finish
	    注：
	      此方法需要单独安装 Git！

	  注：
	    Maven 项目导入到 Eclipse 中后，Eclipse 自动在项目目录下生成了 .classpath、.project、.settings。
	    可见，Eclipse 特定文件确实是不用加入版本控制的！
