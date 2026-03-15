# Concepts
**Full Backup 完全备份**：存完整的信息
**Differential Backup 差异备份**：存和*上次完全备份*的不同信息，即使距离上次完全备份中还间隔很多备份
	备份数据量逐渐累积，但恢复时只需要最后一次完全备份和最后一次差异备份
**Incremental Backup 增量备份**：存和*上次备份*的不同信息
	备份数据量较小速度快，但恢复时需要所有的增量备份和最后一次完全备份
**On-Demand Backup 按需备份**：手动操作备份，和上述三种可重叠
**Snapshot-based Version Control 快照版本控制**: 
	没变的文件不会重新存，只存指向旧对象的引用。
	变了的部分才创建新对象。

```bash
diff v1.c v2.c # generate a diff.txt
patch v1.c diff.txt # generate v2.c
patch -R v2.c diff.txt # generate v1.c
```
## Tools Comparison
单人本地，增量备份：RCS Revision Control System
多人集中式版本控制，增量备份：CVS Concurrent Version System --> SVN Subversion
多人分布式快照版本控制：Git
# Git
**分支引用branch ref**: 指向commit的指针
## Config
```bash
# System config
[/c/Program\ Files/]Git/etc/gitconfig
## 兼容换行符：Mac/Linux使用换行符LF来结束一行，Windows使用回车+换行符CRLF来结束行。
## CR Carriage Return; LF Line Feed
git config --system core.autocrlf true # For Windows: LF->CRLF upon checkout, CRLF->LF upon commit
git config --system core.autocrlf input # For Mac/Linux: CRLF->LF upon commit

# User config
~/.gitconfig
## necessary for submitting code
git config --global user.name
git config --global user.email
## encoding
git config --global gui.encoding utf-8 # Git图形界面使用utf-8
git config --global i18n.commitencoding utf-8 # commit encoding
git config --global i18n.logoutputencoding utf-8 # checkout encoding
git config --global core.quotepath false # 避免非ASCII字符在路径中乱码

# Repo config (default)
working_dir/.git/config
git config --local remote.origin.url
```
### Verification
#### HTTP/HTTPS
```bash
# 口令缓存
git config --global credential.helper store
# HTTPS证书信任
git config --local http.sslverify false
```
#### SSH (more secure)
Put SSH key to the remote platform: [[SSH & SCP]]
## Commands
**Working Tree/Working Directory, Stage/Index(`.git/index`), Remote Repository**
> Note: `checkout` is a terrible command, consider using `switch` and `restore` 
### Start
```bash
# local
git init

# remote
git clone <gitLink>
git lfs clone <gitLink> # lfs插件提供了对二进制文件的区别管理
```
### Info
```bash
git status

git diff A B # nodes or branches
git diff
	--cache # compare current index and last submit
	--name-status # only file names

git log --all --decorate --oneline --graph # commit history
	--name-status # show files edited
	-p -- <fileName> # change history of a single file
	-<number> # show a certain number of latest commits

git show <commit> # see detail info of a commit including parents

git reflog # 分支引用（refs）的位置变动历史
git reset HEAD@{1} # 万能后悔药

git blame -L 10,20 main.py # 看10-20行是谁什么时候写的
```
### Remote Repo
```bash
git remote -v # list all remote repositories
git remote add/set-url <name | origin> <githubLink> # add/change remote repository link

git push origin branch_name:new_branch_name
git push origin :branch_name # delete remote branch
# delete invalid rederence to a remote branch
git remote prune origin
# manually delete
git branch -r -d branch_name

git fetch + git rebase/merge = git pull
git pull origin <remote_branch>:<local_branch>

git tag <tagName> <commit (default HEAD)> -m <message>
git tag v1.0.0 -m "first release"
git tag -d <tagName>
```
### Local Repo
`git add <filePath>` move changes to staging area
`git commit -m <message>` commit
	`-a` git add, then commit
	`git commit --amend` edit last commit message
`git stash`
    `git stash save <stash_name>`
    `git stash apply <stash_name>`
    `git stash list`
    `git stash drop`
#### Commit Nodes
`HEAD`: current commit
`HEAD~n`: the commit n-commits before the current one
`HEAD^n`: the nth parent commit of the current one
	`HEAD^^` is the same as `HEAD~2`, not the same as `HEAD^2`
A..D: commits of B C D
#### Undo
`git reset <commit>` Move back right after the commit specified
    (default) `--mixed` all later commits and changes in your current index will become unstaged changes  
    `--soft` all later commits and changes in your current index will be in stage  
    `--hard` discard everything and go to the specified commit

`git checkout .` discard unstaged changes in working tree
	`git checkout -- <filename>` discard certain file change in current working directory
`git restore <filePath>` discard unstaged changes in working tree
`git restore --staged <filePath>` unstage a file

`git revert <commit>` Make a new commit that reverts back to the commit specified
#### Tracking Issues
`.gitignore` ignores specified untracked file
`git rm = rm + git add`: 
	`-n` dry run
	`--cached` keep the file in the working tree
	``git rm `git ls-files -i --exclude-from=.gitignore` `` remove files in `.gitignore`, but still existing remotely
`git clean -df`, delete untracked files, and `-d` directories
	`git clean -n` for dry run
`git mv ~= mv + git add`，Git 会自动记录并管理这个移动或重命名操作，从而避免额外的步骤。
### Branch Operations
- `git branch` show local branches
	- `git branch -r` show remote branches
	- `git branch -a` show all branches
- `git checkout/switch <branchName>` switch branch
- `git checkout -b <branchName>` create & switch branch
- `git branch -d <branchName>` delete branch
- `git branch -m <newName>` change current branch name
#### Merge
>Merge is best used when the target branch is supposed to be shared. Merge preserves history.

`git checkout B`,`git merge A` B接受A的变化
- 找出分支A, B的共同祖先merge base
- Three‑Way Merge
	- 比较分支A，B和merge base的变化，然后合并生成一个新的合并commit，它有两个父节点：一个来自 A (HEAD^1)，一个来自 B (HEAD^2)
Conflict: 同一行同时被A和B修改，最多一次冲突
	incoming change: A
	current change: B
#### Rebase
>Rebase is best used when the target branch is private. Rebase rewrites history.

`git rebase A B` B迁移到A上后，接受A的变化，B默认为HEAD
* 找出分支A, B的共同祖先merge base
* B上的commit一个接一个rebase上去
Conflict: 下一个要应用的B的commit和已经rebase的commit修改了同一行，最多冲突次数为B要rebase的commit数量
	incoming change: 下一个要应用的B的commit
	current change: 已经rebase到A上的commit
#### Interactive Rebase
`git rebase -i <commit>`
    Can: reorder, omit, squash, edit commits
    Will have a pop-up window that contains commits between HEAD and `commit`
    Conflict: Current is the `branch_to_rebase_onto` (code already in the branch history), Incoming is `the_branch_to_be_rebased` (new local changes)
#### Cherry pick
`git cherry-pick <commit1> <commit2> ...`
	Copy `<commit1> <commit2> ...` and commits to current HEAD
## Industrial tools
Git client: SrouceTree, GitKraken, Github Desktop, TortoiseGit
Git platform: Bitbucket, Github

Github
Avoid entering personal access token every time:
`git config --global credential.helper store`
## PR Comment Priority (Courtesy)
MUST : 必ず直すべき
IMO : 自分なら直すけどどう? 緩やかな指摘 (In my opinion)
IMHO : 丁寧なIMO (In my humble opinion)
nits : 細かい指摘 (nitpick)
Optional
FYI: For your information