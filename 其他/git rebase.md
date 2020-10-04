### 本文介绍
本文通过对比 git merge \<branch\> 和 git rebase \<branch\> 来介绍  git rebase \<branch\> 
	
### 实验对比及分析
	1. 准备本地仓库 Rep
	  master 分支： v1 ← v2_master ← v3_master
	  dev 分支：v1 ← v2_dev ← v3_dev

	  注：其中 v1 是 master 和 dev 公共的历史版本

	  $ git init
	  $ vim 1.txt
	  share
	  $ git add -A
	  $ git commit -m "v1"

	  $ git branch dev

	  $ vim 1.txt
	  share
	  master1
	  $ git add -A
	  $ git commit -m "v2_master"

	  $ vim 1.txt
	  share
	  master1
	  master2
	  $ git add -A
	  $ git commit -m "v3_master"

	  $ git switch dev

	  $ vim 1.txt
	  share
	  dev1
	  $ git add -A
	  $ git commit -m "v2_dev"
	
	  $ vim 1.txt
	  share
	  dev2
	  $ git add -A
	  $ git commit -m "v3_dev"

	  $ git switch master

	2. 将本地仓库 rep 复制 2 份 rep1、rep2，分别测试 merge、rebase

	3. 在 rep1 上测试 git merge <branch>
	  Admin@DESKTOP-JETUSOG MINGW64 ~/Desktop/rep1 (master|MERGING)
	  $ git merge dev
	  Auto-merging 1.txt
	  CONFLICT (content): Merge conflict in 1.txt
	  Automatic merge failed; fix conflicts and then commit the result.

	  Admin@DESKTOP-JETUSOG MINGW64 ~/Desktop/rep1 (master|MERGING)
	  $ vim 1.txt
	
	  Admin@DESKTOP-JETUSOG MINGW64 ~/Desktop/rep1 (master|MERGING)
	  $ git add -A
	
	  Admin@DESKTOP-JETUSOG MINGW64 ~/Desktop/rep1 (master|MERGING)
	  $ git commit -m "resolve conflict"
	  [master 7f4774b] resolve conflict
	
	  Admin@DESKTOP-JETUSOG MINGW64 ~/Desktop/rep1 (master)
	  $ git log --oneline --graph
	  *   7f4774b (HEAD -> master) resolve conflict
	  |\
	  | * 0d9d74a (dev) v3_dev
	  | * 51d652b v2_dev
	  * | 143d9f7 v3_master
	  * | 0980465 v2_master
	  |/
	  * e4e8a16 v1

	4. 在 rep2 测试 git rebase <branch>
	  Admin@DESKTOP-JETUSOG MINGW64 ~/Desktop/rep2 (master)
	  $ git rebase dev
	  error: could not apply 0980465... v2_master
	  Resolve all conflicts manually, mark them as resolved with
	  "git add/rm <conflicted_files>", then run "git rebase --continue".
	  You can instead skip this commit: run "git rebase --skip".
	  To abort and get back to the state before "git rebase", run "git rebase --abort".
	  Could not apply 0980465... v2_master
	  Auto-merging 1.txt
	  CONFLICT (content): Merge conflict in 1.txt
	
	  Admin@DESKTOP-JETUSOG MINGW64 ~/Desktop/rep2 (master|REBASE 1/2)
	  $ vim 1.txt
	  share
	  <<<<<<< HEAD
	  dev1
	  dev2
	  =======
	  master1
	  >>>>>>> 0980465... v2_master

	  修改为：

	  share
	  dev1 master1
	  dev2
	
	  Admin@DESKTOP-JETUSOG MINGW64 ~/Desktop/rep2 (master|REBASE 1/2)
	  $ git add 1.txt
	  
	  Admin@DESKTOP-JETUSOG MINGW64 ~/Desktop/rep2 (master|REBASE 1/2)
	  $ git rebase --continue
	  [detached HEAD 90aa97d] new v2_master
	   1 file changed, 1 insertion(+), 1 deletion(-)
	  error: could not apply 143d9f7... v3_master
	  Resolve all conflicts manually, mark them as resolved with
	  "git add/rm <conflicted_files>", then run "git rebase --continue".
	  You can instead skip this commit: run "git rebase --skip".
	  To abort and get back to the state before "git rebase", run "git rebase --abort".
	  Could not apply 143d9f7... v3_master
	  Auto-merging 1.txt
	  CONFLICT (content): Merge conflict in 1.txt
	
	  Admin@DESKTOP-JETUSOG MINGW64 ~/Desktop/rep2 (master|REBASE 2/2)
	  $ vim 1.txt
	  share
	  <<<<<<< HEAD
	  dev1 master1
	  dev2
	  =======
	  master1
	  master2
	  >>>>>>> 0980465... v2_master

	  修改为：

	  share
	  dev1 master1
	  dev2 master2

	  Admin@DESKTOP-JETUSOG MINGW64 ~/Desktop/rep2 (master|REBASE 2/2)
	  $ git add 1.txt
	
	  Admin@DESKTOP-JETUSOG MINGW64 ~/Desktop/rep2 (master|REBASE 2/2)
	  $ git rebase --continue
	  [detached HEAD f9571b4] new v3_master
	   1 file changed, 1 insertion(+), 1 deletion(-)
	  Successfully rebased and updated refs/heads/master.
	
	  Admin@DESKTOP-JETUSOG MINGW64 ~/Desktop/rep2 (master)
	  $ git log --oneline --graph
	  * f9571b4 (HEAD -> master) new v3_master
	  * 90aa97d new v2_master
	  * 0d9d74a (dev) v3_dev
	  * 51d652b v2_dev
	  * e4e8a16 v1

	  git rebase dev 之前的本地仓库：
	    master 分支： v1 ← v2_master ← v3_master
	    dev 分支：v1 ← v2_dev ← v3_dev

	  git rebase dev 之后的本地仓库：
	    master 分支： v1 ← v2_dev ← v3_dev ← new v2_master ← new v3_master
	    dev 分支：v1 ← v2_dev ← v3_dev

	  master 分支的变化过程，逻辑上可以这么理解：

	  (1) master 分支： v1 ← v2_master ← v3_master

	  (2) master 分支： v1 ← v2_dev ← v3_dev

	  (3) master 分支： v1 ← v2_dev ← v3_dev ← new v2_master
	    其中 new v2_master 版本是由 v2_master 版本和 v3_dev 版本合并得到的

	  (4) master 分支： v1 ← v2_dev ← v3_dev ← new v2_master ← new v3_master
	    其中 new v3_master 版本是由 v3_master 版本和 new v2_master 版本合并得到的