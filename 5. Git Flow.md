## 本文介绍
	Git 工作流：使用 Git 进行项目开发的工作流程
	本文介绍一种 Git 工作流：Git Flow

## 常设分支
### Master 分支
	概念：Master 分支用于发布所有提供给用户使用的正式版本
	命名：代码库有且仅有一个 Master 分支，可以命名为 master
	注：
	  Master 分支要时刻与远程同步
	
	Master
	  ↓
	  ● Tag 0.1
	  ↓
	  ● Tag 0.2
	  ↓
	  ● Tag 0.3
	
### Develop 分支
	概念：从 Master 分支上面分出来，用于日常开发、发布最新隔夜版本。如果想正式对外发布，就并入 Master 分支
	命名：代码库有且仅有一个 Develop 分支，可以命名为 develop
	注：
	  团队所有成员都需要在 Develop 分支上工作，所以需要与远程同步

	1. 从 Master 分支创建并切换至 Develop 分支
	  $ git switch master
	  $ git switch -c develop

	2. 日常开发，发布最新隔夜版本

	3. 将 Develop 分支合并到 Master 分支
	  $ git switch master
	  $ git merge --no-ff develop

	4. 继续日常开发...

	Develop   Master
	   ↓
	   ●        ↓
	   ↓    ↘
	   ●        ● Tag 0.1
	   ↓
	   ●        ↓
	   ↓    ↘
	   ●        ● Tag 0.2
	   ↓
	   ●        ↓
	   ↓    ↘
	   ●        ● Tag 0.3
	
	注：图中省略了 Develop 和 Master 之间的 Release 分支

## 临时分支
### Hotfix 分支
	概念：从 Master 分支上面分出来的，用于修复 bug，修复完成后，要再并入 Master 和 Develop 分支，最后删除 Hotfix 分支。
	命名：hotfix-xxx，其中 xxx 是 bug 代号
	注：
	  Hotfix 分支只用于在本地修复 bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个 bug

	比如说，现在要修复代号为 0.1 的 bug，工作流程如下：

	1. 从 Master 分支创建并切换至 Hotfix 分支
	  $ git switch master
	  $ git switch -c hotfix-0.1

	2. 经历若干个提交，修复完成

	3. 将 Hotfix 分支合并到 Master 分支
	  $ git switch master
	  $ git merge --no-ff hotfix-0.1
	  $ git tag -a 0.1.1

	4. 将 Hotfix 分支合并到 Develop 分支
	  $ git switch develop
	  $ git merge --no-ff hotfix-0.1

	5. 删除 Hotfix 分支
	  $ git branch -d hotfix-0.1

	Develop Hotfix Master

	   ↓

	   ●             ↓
    
	   ↓      ↘

	   ●             ● Tag 0.1
	             ↙
       ↓      ●
	          ↓ 
	   ●      ●      ↓   

	   ↓   ↙      ↘

	   ●             ● Tag 0.1.1

	   ↓      
	                
       ●

	注：图中省略了 Develop 和 Master 之间的 Release 分支

### Feature 分支
	概念：从 Develop 分支上面分出来的，用于开发某种特定功能，开发完成后，要再并入 Develop 分支，最后删除 Feature 分支。
	命名：feature-xxx，其中 xxx 是功能代号
	注：
	  * 不同功能的开发在对应的分支里被并行推进
	  * Feature 分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发
	
	比如说，现在要开发代号为 vulan 的功能，工作流程如下：

	1. 从 Develop 分支创建并切换至 Feature 分支
	  $ git switch develop
	  $ git switch -c feature-vulcan

	2. 经历若干个提交，开发完成

	3. 将 Feature 分支合并到 Develop 分支
	  $ git switch develop
	  $ git merge --no-ff feature-vulan

	4. 删除 Feature 分支
	  $ git branch -d feature-vulan

	如果中途放弃开发，使用 $ git branch -D <branch> 删除没有被合并的分支即可。

	Feature   Develop
	            ↓
	            ●
	       ↙    ↓
	   ●        ● 
	   ↓        ↓
	   ●        ● 
	   ↓        ↓
	   ●        ●
	       ↘    ↓
	            ●
	            ↓
	            ●

### Release 分支
	概念：从 Develop 分支上面分出来的，用于预发布正式版本。确认没有问题，预发布结束。然后合并进 Develop 和 Master 分支，发布正式版本，最后删除 Release 分支。
	命名：release-xxx，其中 xxx 是版本代号
	
	比如说，现在要预发布版本 0.2，工作流程如下：

	1. 从 Develop 分支创建并切换至 Release 分支
	  $ git switch master
	  $ git switch -c release-0.2

	2. 测试后，确认没有问题
	  注：如果发现 bug，一般就在 Release 分支上进行修复。

	3. 将 Release 分支合并到 Master 分支
	  $ git switch master
	  $ git merge --no-ff release-0.2
	  $ git tag -a 0.2

	4. 将 Release 分支合并到 Develop 分支
	  $ git switch develop
	  $ git merge --no-ff release-0.2

	5. 删除 Release 分支
	  $ git branch -d release-0.2

	Develop   Release   Master

	   ↓

	   ●                  ↓
    
	   ↓

	   ●                  ● Tag 0.1
	       
       ↓    ↘
	            ● Bug
	   ●        ↓         ↓   
	            ● Pass
	   ↓         
	       ↙         ↘
	   ●                  ● Tag 0.2

	   ↓      
	                
       ●

## 简单总结
	                           Master （小版本更新）
	                         ↗
	修复 bug：Master → Hotfix
	                         ↘
	                           Develop
	                                                 Master （大版本更新）
	                                               ↗
	开发功能：Develop → Feature → Develop → Release
	                                               ↘
	                                                 Develop