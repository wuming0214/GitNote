### 本文介绍
	我想知道 git push 不带任何参数时的作用。

	git push 的作用与 push.default 的设置有关，需要分情况讨论。

	本文只看 push.default 取 simple、upsteam、current 时的情况。

	假设有如下远程仓库和克隆下来的本地仓库：

	远程仓库：
	  master
	  ↓
	  c1
	  ↑
	  dev

	本地仓库：
	  master
	  ↓
	  c1
	  ↑
	  dev

	注：
	  * 克隆操作只创建了 master 分支，通过 $ git checkout origin/dev 和 $ git branch dev 创建本地的 dev 分支
	  * 克隆操作默认设置了本地 master 分支的上游分支 origin/master，通过 $ git branch --unset-upstream 取消
   
### git push 的作用
#### push.default = simple
	自从 Git 2.0，push.default 默认就是 simple，不需要设置

	$ git push：
	1. 若本地分支未设置上游分支
	  a. 当存在名为 origin 远程仓库时，提示错误：
	    fatal: The current branch master has no upstream branch.
	    To push the current branch and set the remote as upstream, use
	
	        git push --set-upstream origin master

	  b. 当不存在名为 origin 远程仓库时，提示错误：
	    fatal: No configured push destination.
	    Either specify the URL from the command-line or configure a remote repository using
	
	        git remote add <name> <url>
	
	    and then push using the remote name
	
	        git push <name>

	  总之，当前分支未设置上游分支时，Git 不支持 $ git push 这种用法！

	2. 若本地分支设置了上游分支，将 master 的上游分支设置成 origin/master，dev 的上游分支设置成 origin/dev
	  将当前分支的内容推送到上游分支

	3. 若本地分支设置了上游分支，将 master 的上游分支设置成 origin/dev，dev 的上游分支设置成 origin/master
	  fatal: The upstream branch of your current branch does not match
	  the name of your current branch.  To push to the upstream branch
	  on the remote, use
	
	      git push origin HEAD:dev
	
	  To push to the branch of the same name on the remote, use
	
	      git push origin HEAD
	
	  To choose either option permanently, see push.default in 'git help config'.

	  由于当前分支与上游分支不同名，Git 拒绝了推送操作！

	  根据提示，
	    使用 git push origin HEAD:dev 可以将当前分支（master）推送到上游分支 origin/dev；
	    使用 git push origin HEAD 可以将当前分支推送到 origin 上的同名分支 origin/master！

	  
#### push.default = upstream
	执行 $ git config push.default upstream 设置 push.default

	$ git push：
	1. 若本地分支未设置上游分支
	  a. 当存在名为 origin 远程仓库时，提示错误：
	    fatal: The current branch master has no upstream branch.
	    To push the current branch and set the remote as upstream, use
	
	        git push --set-upstream origin master

	  b. 当不存在名为 origin 远程仓库时，提示错误：
	    fatal: No configured push destination.
	    Either specify the URL from the command-line or configure a remote repository using
	
	        git remote add <name> <url>
	
	    and then push using the remote name
	
	        git push <name>

	  总之，当前分支未设置上游分支时，Git 不支持 $ git push 这种用法！

	2. 若本地分支设置了上游分支
	  将当前分支的内容推送到上游分支

#### push.default = current
	执行 $ git config push.default current 设置 push.default

	$ git push：将当前分支的内容推送到 origin 仓库的同名分支

	注：
	  如果没有名为 origin 的仓库，将提示如下错误：
	  fatal: No configured push destination.
	  Either specify the URL from the command-line or configure a remote repository using
	
	      git remote add <name> <url>
	
	  and then push using the remote name
	
	      git push <name>

	更准确的说法在 Git 命令手册中（通过 $ git help push 查看）：
	  When the command line does not specify where to push with the <repository> argument, 
	  branch.*.remote configuration for the current branch is consulted to determine where to push. 
	  If the configuration is missing, it defaults to origin.

	现在我想测试一下文档中的描述。

	执行 $ git config branch.master.remote origin3，

	此时 .git/config 新增了这么一段
	[branch "master"]
		remote = origin3

	在当前分支下执行 $ git push，发现此时 $ git push 是将当前分支 master 推送到 origin3 仓库的同名分支。

### 总结
	|  push.default   | $ git push  |
	|  ----  | ----  |
	| simple  | 将当前分支推送到同名上游分支 |
	| upstream  | 将当前分支推送到上游分支 |
	| current  | 将当前分支推送到 origin 仓库（可以设置）的同名分支 |
