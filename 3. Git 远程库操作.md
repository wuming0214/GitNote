### Git 远程库操作命令
#### 下载
	• git clone <远程仓库地址>：克隆远程仓库到本地
	  1. 下载所有版本到本地
	
	  2. 设置 origin 为远程仓库地址的别名
	    $ git remote add origin <远程仓库地址>
	
	  3. 纪录远程仓库各个分支的最新提交以及当前分支
	    创建指针，命名为 origin/HEAD，用于纪录远程仓库 origin 中的当前分支
	    创建指针，命名为 origin/master，用于纪录远程仓库 origin 中的 master 分支上最新提交的哈希值
	    ...
	    注：
	      用于纪录远程仓库 origin 中的 master 分支上最新提交的哈希值的指针被称为远程跟踪分支。
	
	  假设远程仓库的当前活跃分支是 master（可以在仓库主页设置）
	
	  4. 检出远程仓库的当前活跃分支上的最新版本
	    $ git checkout origin/master
	     
	  5. 创建分支 master，用于和远程 master 分支同步
	    $ git branch master
	
	  6. 设置本地分支 master 的上游分支是远程分支 master 
	    $ git branch --set-upstream-to=origin/master
	    注：
	      通过 git status 可以查看本地分支领先上游分支在本地的远程跟踪分支几个提交。
	    
	• git fetch
	  ▪ git fetch <远程仓库名> <远程分支名>：取回远程分支到本地
	  ▪ git fetch：取回所有远程分支到本地

#### 合并    
	• git push：将本地分支合并到远程分支，必须是 Fast-forward 模式，否则合并失败！
	  ▪ git push <远程仓库名> <本地分支名>:<远程分支名>：将本地分支的内容同步至远程分支
	  ▪ git push <远程仓库名> <本地分支名>：将本地分支的内容同步至同名的远程分支
	  ▪ git push：将当前分支的内容同步至上游分支，前提是当前分支必须与上游分支同名，否则拒绝推送
	  注：
	    如果 push 失败，即将本地分支合并到远程分支失败，则需要将远程分支合并到本地分支（此时需要手动合并冲突的文件），再将本地分支合并到远程分支即可。
	    这样做的原因其实是为了把解决冲突的整个过程放在本地，在解决冲突期间不用联网。

	• git pull：将远程分支合并到本地分支
	  ▪ git pull <远程仓库名> <远程分支名>：将远程分支合并到当前分支
	  ▪ git pull：将当前分支的上游分支合并到当前分支

#### 管理远程仓库
	• git remote add <远程仓库名> <远程仓库地址>：添加远程仓库
	• git remote rm <远程仓库名>：删除远程仓库
	• git remote -v：列出远程仓库

#### 设置上游分支
	相关命令：
	  • git branch --set-upstream-to=<远程仓库名/远程分支名>：设置当前分支的上游分支
	  • git branch --unset-upstream：取消设置当前分支的上游分支
	
	信息保存位置：.git/config
	  [branch "master"]
	  	remote = origin
	  	merge = refs/heads/master
	  定义了本地 master 分支的上游分支为 origin 仓库的 master 分支

	  因此，以下两套命令均能设置上游分支
	    $ git config branch.dev.remote origin
	    $ git config branch.dev.merge refs/heads/dev
	    或者
	    $ git switch master
		$ git branch --set-upstream-to=origin/master
	

### Git 远程库操作示例
	任务：在远程仓库的 branch_master 分支上最新版本R的基础上，开发出具有功能 F 的新版本
	步骤：
	  1. 克隆版本库到本地
	    $ git clone https://github.com/wuming0214-1/huashan.git .
	
	  2. 检出版本R
	    $ git checkout origin/branch_master
	
	  3. 创建分支 branch_master (用于和远程的 branch_master 分支同步，两者可以不同名)
	    $ git branch branch_master
	
	  4. 创建分支 branch_F，切换至分支 branch_F，经历多次提交，开发出具有功能 F 的新版本
	    $ git branch branch_F
	    $ git switch branch_F
	
	    $ vim huashanjianfa.txt
	    $ git add huashanjianfa.txt
	    $ git commit -m "append: 我是令狐冲，我比岳不群厉害！"
	
	  5. 合并 branch_F 分支到 branch_master 分支
	    $ git switch branch_master
	    $ git merge branch_F
	
	  6. 将本地分支 branch_master 上内容同步至远程分支 branch_master
	    $ git push origin branch_master:branch_master
	    注：
	      • 期间需要填写用户名和密码登录 Github，因为需要有写远程仓库 origin 的权限！
	      • Windows 系统的凭据管理器会记住登录 Github 的用户名和密码，下次如需使用别的账户登录 Github 需要去凭据管理器删除凭据。
	
	  如果同步失败，说明这段时间远程 branch_master 分支上有了新提交！
	  重复如下 3 步，将功能 F 合并到远程 branch_master 分支的新版本
	
	  7. 将远程 branch_master 分支的内容同步至本地 branch_master 分支
	    $ git fetch origin branch_master
	    $ git reset --hard origin/branch_master
	
	  8. 合并 branch_F 分支到 branch_master 分支
	    $ git merge branch_F
	    Auto-merging huashanjianfa.txt
	    CONFLICT (content): Merge conflict in huashanjianfa.txt
	    Automatic merge failed; fix conflicts and then commit the result.
	
	    $ cat huashanjianfa.txt
	    华山剑法，天下第一！
	    <<<<<<< HEAD
	    我是岳不群，我比令狐冲厉害！
	    =======
	    我是令狐冲，我比岳不群厉害！
	    >>>>>>> branch_F
	
	    $ vim huashanjianfa.txt
	    $ cat huashanjianfa.txt
	    华山剑法，天下第一！
	    岳不群和令狐冲一样厉害！
	    $ git add huashanjianfa.txt
	    $ git commit -m "resolve conflict：岳不群和令狐冲一样厉害！"
	
	  9. 将本地分支 branch_master 上的内容同步至远程分支 branch_master
	    $ git push origin branch_master:branch_master