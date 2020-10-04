## 本地库初始化
	1. 打开 Git Bash
	2. 切换到项目目录
	3. 输入 git init

	提示信息：Initialized empty Git repository in ${项目目录}/.git
	发现在项目目录下创建了一个隐藏文件夹 .git，里面的内容不要乱动。

## 设置签名
	❄ 签名是什么
	  • 签名的组成：签名由用户名和邮件地址组成
	  • 签名的作用：标识开发人员的身份
	  • 辨析：这里设置的签名和登录代码托管中心(比如 github，码云)的账号、密码没有任何关系
	
	❄ 设置签名
	  • 设置仓库级别的签名
	    $ git config user.name tom_pro
	    $ git config user.email tom_pro@gmail.com
	    注：
	      * 设置的签名仅在当前仓库范围内有效
	      * 签名信息保存位置：${项目目录}/.git/config
		    [user]
		        name = tom_pro
		        email = tom_pro@email.com 

	  • 设置系统用户级别的签名
	    $ git config --global user.name tom_glb
	    $ git config --global user.email tom_glb@gmail.com
	    注：
	      * 设置的签名对当前登陆操作系统的用户有效
	      * 签名信息保存位置：~/.gitconfig
	        [user]
	            name = tom_glb
	            email = tom_glb@gmail.com
	
	  注：
	    * 仓库级别的签名优先于系统用户级别的签名
	    * 实际开发中，没必要把两个级别的签名都设置上，设置一个系统用户级别的就够了。

## 设置忽略文件
	1. 编写忽略文件
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
	
	  # Eclipse 特定文件
	  .classpath
	  .project
	  .settings
	  target
	注：
	  可以参考 Github 提供的 .gitignore 模板集合：https://github.com/github/gitignore，里面有用于 Java 的 gitignore 模版。

	2. 设置忽略文件
	  $ git config --global core.excludesfile ~/Java.gitignore
	  注：
	    信息保存位置：~/.gitconfig
	    [core]
	    	excludesfile = ~/Java.gitignore

## 基本操作
	❄ 工作区操作
	  • 文件新建：vim good.txt
	  • 文件删除：rm good.txt
	  • 文件修改：vim good.txt
	  • 文件查看：cat good.txt
	
	❄ 工作区 → 暂存区
	  • git add good.txt

	❄ 提交
	  • git commit
	    将暂存区的内容提交到版本库，产生一个新的项目版本。期间会打开 git 的默认编辑器，输入提交说明即可。
	
	  • git commit -m "My second commit, modify good.txt"
	    功能同上，直接通过 -m 参数指定提交说明。

	❄ 查看状态
	  • git status
	    ▪ 当前所处分支
	    ▪ 版本库是否为空
	    ▪ 暂存区相对当前分支上最新版本的文件变化：新建、修改、删除
	    ▪ 工作区相对暂存区的文件变化：新建、修改、删除

	❄ 文件恢复
	  ☛ 暂存区 → 工作区
	    • git restore good.txt

	  ☛ HEAD 版本 → 暂存区
	    • git restore --staged good.txt

	❄ 文件比较
	  ☛ 暂存区 -?→ 工作区
	    • git diff good.txt：查看工作区 good.txt 相对暂存区 good.txt 做了哪些改动
	    • git diff：查看工作区相对暂存区做了哪些改动

	  ☛ HEAD 版本 -?→ 暂存区
	    • git diff --cached good.txt：查看暂存区 good.txt 相对 HEAD 版本 good.txt 做了哪些改动
	    • git diff --cached：查看暂存区相对 HEAD 版本做了哪些改动
	
	  ☛ HEAD 版本 -?→ 工作区
	    • git diff HEAD good.txt：查看工作区 good.txt 相对 HEAD 版本 good.txt 做了哪些改动
	    • git diff HEAD：查看工作区相对 HEAD 版本做了哪些改动

	  使用示例：
	    $ git diff good.txt
	    @@ -3,6 +3,8 @@
	     good3
	     good4
	     good5
	    +bad1
	    +bad2
	     good6
	     good7
	     good8
	    @@ -12,7 +14,7 @@
	     good12
	     good13
	     good14
	    -good15
	    +good15@@@@@@
	     good16
	     good17
	     good18
		  
	    解释：
	      第一处修改：
	        ▪ 在哪修改的？
	          @@ -3,6 +3,8 @@ 表明在暂存区 good.txt 的第 3 ~ 3+6 行之间做了修改。
	
	        ▪ 怎么修改的？
	          1. 在第 5 行后插入一行 bad1
	          2. 再插入一行 bad2
	
	      第二处修改：
	        ▪ 在哪修改的？
	          @@ -12,7 +14,7 @@ 表明在暂存区 good.txt 的第 12 ~ 12+7 行之间做了修改。
	
	        ▪ 怎么修改的？
	          1. 先删除 第 12 行的 good15
	          2. 再插入一行 good15@@@@@@

	❄ 版本库
	  ☛ 版本查看
	    • git log：查看当前分支上的提交记录
	      使用示例：
	        $ git log
	        commit cab20c77f2b22e922cbd7c4f0848c363bc04e10b (HEAD -> master)
	        Author: tom_glb <tom_glb@gmail.com>
	        Date:   Thu Sep 10 20:36:07 2020 +0800
	
	            insert zzzzzz
	
	        commit 88cd837366e48159c6867d68627bcbe7c2f63e28
	        Author: tom_glb <tom_glb@gmail.com>
	        Date:   Thu Sep 10 20:35:39 2020 +0800
	
	            insert yyyyyy
	
	        commit c317315751c922f8ab2b73bd319291fed7209d46
	        Author: tom_glb <tom_glb@gmail.com>
	        Date:   Thu Sep 10 20:35:14 2020 +0800
	
	            insert xxxxxx
				  
	        解释：
	          ▪ 8c23e45f0cd6a0e947178bf3394757882780625f 是 commit id，可以理解为版本号
	          ▪ HEAD -> master 表示当前处于 master 分支，并且此提交是分支上的最新提交

	    • git log --pretty=oneline：查看当前分支上的提交记录，每条日志一行，显示完整版本号
	      使用示例：
	        $ git log --pretty=oneline
	        cab20c77f2b22e922cbd7c4f0848c363bc04e10b (HEAD -> master) insert zzzzzz
	        88cd837366e48159c6867d68627bcbe7c2f63e28 insert yyyyyy
	        c317315751c922f8ab2b73bd319291fed7209d46 insert xxxxxx

	    • git log --oneline: 查看当前分支上的提交记录，每条日志一行，只显示部分版本号
	      使用示例：
	        $ git log --oneline
	        cab20c7 (HEAD -> master) insert zzzzzz
	        88cd837 insert yyyyyy
	        c317315 insert xxxxxx

	    • git reflog: 查看 HEAD 引用纪录
	      使用示例：
	        $ git reflog
	        cab20c7 (HEAD -> master) HEAD@{0}: commit: insert zzzzzz
	        88cd837 HEAD@{1}: commit: insert yyyyyy
	        c317315 HEAD@{2}: commit: insert xxxxxx

	        解释：
	          ▪ HEAD@{2} means "where HEAD used to be two moves ago"
			
	    注：以上命令的打印输出若一屏显示不下，按 b 向上翻页，按 空格 向上翻页，按 q 退出
			

	  ☛ 版本切换
	    • 基于异或符号实现版本后退
	      1. 使用 git log --oneline 查看提交记录，好确定后退至哪个版本
	      2. 版本后退
	        git reset --hard HEAD^：从 HEAD 版本开始，后退 1 个版本
	        git reset --hard HEAD^^：从 HEAD 版本开始，后退 2 个版本
	        ...
	
	    • 基于波浪线符号实现版本后退
	      1. 使用 git log --oneline 查看提交记录，好确定后退至哪个版本
	      2. 版本后退
	        git reset --hard HEAD~n：从 HEAD 版本开始，后退 n 个版本
	
	    • 基于版本号实现版本切换
	      ▪ 版本后退：
	        1. 使用 git log 查看提交记录，好确定切换至哪个版本
	        2. 使用 git reset --hard 版本号
	
	      ▪ 版本前进：
	        1. 使用 git reflog 查看 HEAD 引用记录，好确定切换至哪个版本
	        2. 使用 git reset --hard 版本号
	
	      注：版本号没必要写全，写前几位就可以了，Git 会自动去找。
	
	    补充：
	      ▪ git reset --soft <commit>
		    ✓ resets the current branch head to <commit>
		    ✗ resets the index to the tree of <commit>
		    ✗ update the working tree
	
	      ▪ git reset --mixed <commit>
		    ✓ resets the current branch head to <commit>
		    ✓ resets the index to the tree of <commit>
		    ✗ update the working tree
	
	      ▪ git reset --hard <commit>
		    ✓ resets the current branch head to <commit>
		    ✓ resets the index to the tree of <commit>
		    ✓ update the working tree
			
	使用示例：
	  $ git init

	  $ git status
	  On branch master
	  No commits yet
	  nothing to commit (create/copy files and use "git add" to track)

	  $ vim good.txt
	  aaaaaa
	  bbbbbb
	  cccccc

	  $ git status
	  On branch master
	  No commits yet
	  Untracked files:
	  (use "git add <file>..." to include in what will be commited)
		good.txt
	  nothing added to commit but untracked files present (use "git add" to track)

	  $ git add good.txt
	  warning: LF will be replaced by CRLF in good.txt.
	  The file will have its original line endings in your working directory  

	  输出解释：
	    第一句：good.txt 中的 LF 会被替换为 CRLF 再添加到暂存区。
	    第二句：工作区的原文件的行结尾保持不变，依然是 LF。
	  
	  为什么会出现警告？
	    因为在安装 git 时配置了行尾风格转换：Checkout Windows-style, commit Unix-style endings.
	    即：
	      Git will convert LF to CRLF when checking out text files.
	      When commiting text files, CRLF will be converted to LF.

	    在这种配置下，工作区的文件都应该使用 CRLF 作为行尾，提交时 Git 自动转换为 LF，检出时自动转换为 CRLF。
	    可是现在工作区的 good.txt 行尾采用的是 LF，因此会出现此警告。如果good.txt 是采用的行结尾是 CRLF，将不会有警告。

	    注：尽管是在 Windows上安装的 Git，但是由于 good.txt 是在 Git Bash 中使用 vim 编辑器创建出来的，因此 good.txt 中的行结尾是 LF。

	  $ git status
	  On branch master	
	  No commits yet
	  Changes to be commited:
	    (use "git rm --cached <file>..." to unstage)
		  new file: good.txt

	  $ git commit
	  [master (root-commit) 1d3d6af] My first commit, new file good.txt
	   1 file changed, 3 insertions(+)
	  create mode 100644 good.txt
	  
	  输出解释：
	    root-commit：根提交，即版本库初始化后的第一次提交。
	    1d3d6af: commit id，版本号
	    100644：100 表示文件类型是普通文件，644 代表文件权限
	  
	  $ git status
	  On branch master
	  nothing to commit, working tree clean
				  
	  $ vim good.txt（添加了一行）
	  aaaaaa
	  bbbbbb
	  cccccc
	  dddddd
	  
	  $ git status
	  On branch master
	  Changes not staged for commit:
	    (use "git add <file>..." to update what will be commited)
	    (use "git restore <file>..." to discard changes in working directory)
		  modified:    good.txt
	  
	  no changes added to commit (use "git add" and/or "git commit -a") 
	  
	  $ git add good.txt
	  warning: LF will be replaced by CRLF in good.txt.
	  The file will have its original line endings in your working directory

	  $ git status
	  On branch master
	  Changes to be commited:
	    (use "git restore --staged <file>..." to unstage)
		  modified:    good.txt
	  
	  $ git commit -m "My second commit, modify good.txt"
	  [master 1acccc9] My second commit, modify good.txt
	   1 file changed, 1 insertion(+)

## Git 分支管理
### 理解分支
    一个分支对应一个任务！
	
	任务：
	  A，B 在 branch_master 最新版本的基础上开发出具有功能 fun1，fun2 的新版本。
	  
    做法：
	  1. 切换至 branch_master 分支
      2. 领取任务，创建分支
        • A 领取任务，创建分支 branch_a。
	    • B 领取任务，创建分支 branch_b。

      3. 切换分支，推进任务
        A，B 分别切换至 branch_a，branch_b 分支上，并行推进，互不影响。
  
      4. 完成任务，合并分支
        (1) 先是 A 在 branch_a 上完成了任务，实现了功能 fun1，然后将 branch_a 上的修改合并到 branch_master，
	    使得 branch_master 分支上的版本具有功能 fun1。
        (2) 接着 B 在 branch_b 上完成了任务，实现了功能 fun2，然后将 branch_b 上的修改合并到 branch_master，
	    与 A 所做的修改进行自动合并 or 手动合并，使得 branch_master 分支上的版本同时具有功能 fun1、fun2。

      A 完成任务 task1, B 完成任务 task2，成果合并，两个任务被完成了。

### 分支操作
	• 创建分支：git branch <branch>
	• 删除分支：git branch -d <branch>
	• 列出分支（带 * 的是当前分支）：git branch -v
	• 切换分支：git switch <name> or git checkout <branch>
	• 合并分支（到当前分支）：git merge <branch>，推荐使用 git merge --no-ff <branch>

	使用示例：
	  1. 准备工作
	    $ git init
		
	    $ vim good.txt
	    good
		good
		good
		good

	    $ git add good.txt
		
	    $ git commit -m "new good.txt"
		
	    $ git branch -v
	    * master dd09329 new good.txt
		
	  2. 领取任务，创建分支
	    $ git branch hot_fix
		
	  3. 切换分支，推进任务
	    $ git switch hot_fix
	    Switched to branch 'hot_fix'
		
	    $ vim good.txt
	    good
	    good
	    good edit by hot fix
	    good

	    $ git add good.txt
		
	    $ git commit -m "test branch"
		
	  4. 完成任务，合并分支
	    $ git switch master
	    Switched to branch 'master'
		
	    $ git merge hot_fix
	    Updating dd09329..0fbad9a
	    Fast-forward
	     good.txt | 2 +-
	     1 file changed, 1 insertion(+), 1 deletion(-)
		
	    $ cat good.txt
	    good
	    good
	    good edit by hot_fix
	    good

	  5. 删除分支
	    $ git branch -d hot_fix
	    Deleted branch hot_fix (was 0fbad9a).

### 解决冲突
	1. 冲突的产生
	  接着 "理解分支" 部分里第 4 点来说，

	  (1) 先是 A 在 branch_a 上完成了任务，实现了功能 fun1，然后将 branch_a 上的修改合并到 branch_master，
	  使得 branch_master 分支上的版本具有功能 fun1。
      (2) 接着 B 在 branch_b 上完成了任务，实现了功能 fun2，然后将 branch_b 上的修改合并到 branch_master，
      与 A 所做的修改进行合并，使得 branch_master 分支上的版本同时具有功能 fun1 和 功能 fun2。
	
	  如果 A, B 对同一个文件 good.txt 都有修改，那么 Git 就不知道要想合并之后的版本同时具有功能 fun1、fun2，
	  我应该把 good.txt 合并成什么样子。此时 Git 就以冲突的形式告诉后提交的 B：

	  (1) 自动合并 good.txt 失败，我只能在 good.txt 中标记出 A 提交版本和 B 提交版本不同的地方供你参考。
	  (2) 接下来由你来手动合并 good.txt，你可以选择和 A 商量，然后编辑 good.txt 到双方满意的程度。
	  (3) 最后通过 git add good.txt 表示手动合并 good.txt 完成。最后提交，完成合并分支操作。

	2. 冲突的解决
	  (1) 手动合并所有 Git 不能自动合并的文件
	    a. 编辑文件，删除特殊符号
	    b. 把文件修改到满意的程度，保存退出
	    c. git add <file>，表示手动合并 file 完成
	    d. 重复上述三步手动合并其他 Git 不能自动合并的文件
	  (2) 使用 git commit -m "提交说明" 提交合并后的版本，完成合并分支操作。

	演示：
	  1. 在 master 分支上准备提交记录
	    $ git init

	    $ vim good.txt
	    good
	    good
	    good

	    $ git add good.txt
		
	    $ git commit -m "new good.txt"
		
	  2. 创建分支 hot_fix1, hot_fix2
	    $ git branch hot_fix1
		
	    $ git branch hot_fix2
		
	  3. 在 hot_fix1 上提交一个记录
	    $ git switch hot_fix1
	    Switched to branch 'hot_fix1'
		
	    $ vim good.txt
	    good
	    good edit by hot_fix1
	    good
		
	    $ git add good.txt
		
	    $ git commit -m "edit by hot_fix1"
		
	  4. 合并 hot_fix1 到 master 分支
	    $ git switch master
	    Switched to branch 'master'
		
	    $ git merge hot_fix1
	    Updating 18f9e4c..7eede53
	    Fast-forward
	     good.txt | 2 +-
	     1 file changed, 1 insertion(+), 1 deletion(-)
		
	  5. 在 hot_fix2 上提交一个记录
	    $ git switch hot_fix2
	    Switched to branch 'hot_fix2'
		
	    $ vim good.txt
	    good
	    good edit by hot_fix2
	    good
		
	    $ git add good.txt
		
	    $ git commit -m "edit by hot_fix2"
	    [hot_fix2 e5218f4] edit by hot_fix2
	     1 file changed, 1 insertion(+), 1 deletion(-)
		
	  6. 合并 hot_fix2 到 master 分支
	    $ git switch master
	    Switched to branch 'master'
		
	    $ git merge hot_fix2
	    Auto-merging good.txt
	    CONFLICT (content): Merge conflict in good.txt
	    Automatic merge failed; fix conflicts and then commit the result.
		
	  7. 发现自动合并 good.txt 失败，开始手动合并
	    $ cat good.txt
	    good
	    <<<<<<< HEAD
	    good edit by hot_fix1
	    =======
	    good edit by hot_fix2
	    >>>>>>> hot_fix2
	    good
		
	    $ vim good.txt
	    good
	    good edit by hot_fix
	    good

	    $ git add good.txt
		
	  8. 提交至 master 分支，完成合并分支操作
	    $ git commit -m "resolve conflict"
	    [master e45065d] resolve conflict
		
	  9. 查看 master 分支上提交记录
	    $ git log --oneline --graph
	    *   e45065d (HEAD -> master) resolve conflict
	    |\
	    | * e5218f4 (hot_fix2) edit by hot_fix2
	    * | 7eede53 (hot_fix1) edit by hot_fix1
	    |/
	    * 18f9e4c new good.txt

			
