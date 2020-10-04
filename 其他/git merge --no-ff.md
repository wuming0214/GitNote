### 本文介绍
本文通过对比 git merge \<branch\> 和 git merge --no-ff \<branch\> 来介绍  git merge --no-ff \<branch\>

### 实验对比
	1. 准备本地仓库 rep
	  master 分支： v1
	  dev 分支：v1 ← v2

	  注：其中 v1 是 master 和 dev 公共的历史版本

	  $ git init
	
	  $ touch 1.txt
	  $ git add 1.txt	
	  $ git commit -m "v1"

	  $ git switch -c dev
	
	  $ touch 2.txt
	  $ git add 2.txt
	  $ git commit -m "v2"

	2. 将本地仓库 rep 复制 2 份 rep1、rep2，分别测试 merge、merge --no-ff

	3. 在 rep1 上测试 git merge <branch>
	  $ git switch master
	
	  $ git merge dev
	  Updating 65e9ed0..9c1a604
	  Fast-forward
	   2.txt | 0
	   1 file changed, 0 insertions(+), 0 deletions(-)
	   create mode 100644 2.txt
	
	  $ git log --oneline --graph
	  * 9c1a604 (HEAD -> master, dev) v2
	  * 65e9ed0 v1


	4. 在 rep2 测试 git rebase <branch>
	  $ git switch master
	
	  $ git merge --no-ff dev
	  Merge made by the 'recursive' strategy.
	   2.txt | 0
	   1 file changed, 0 insertions(+), 0 deletions(-)
	   create mode 100644 2.txt
	
	  $ git log --oneline --graph
	  *   5083347 (HEAD -> master) Merge branch 'dev' into master
	  |\
	  | * 9c1a604 (dev) v2
	  |/
	  * 65e9ed0 v1

### 简单总结
	区别：
	  不带 --no-ff 参数，Git 执行“快进式合并”（fast-forward merge），直接将当前分支指向 <branch> 上最新提交。
	  使用 --no-ff 参数，表示禁用快进式合并，此时在当前分支上生成一个新提交，这种方式可以使得版本演进更加清晰。