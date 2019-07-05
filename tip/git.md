```javascript
git stash      // 保存 add 过但未 commit 的文件
git stash -u   // 与git stash 相比会保存未add的文件
git add -p     // add 一个文件的一部分
tig status
git cherry-pick  // 提取某次提交
git push --delete origin branch //删除远程分支
git branch -m oldbranch newbranch  //重命名本地分支
git log --diff-filter=D --summary  //查看被删除的文件
git checkout commitId~1 filename  //恢复被删除的文件，commitId为上个命令看到的被删除文件所在的commitID
cd ~/.oh-my-zsh/plugins/git 查看别名配置
alias gcmsg // gcmsg='git commit -m'

当我们需要删除暂存区或分支上的文件, 同时工作区也不需要这个文件了, 可以使用

git rm file_path
git commit -m 'delete somefile'
git push

当我们需要删除暂存区或分支上的文件, 但本地又需要使用, 只是不希望这个文件被版本控制, 可以使用

git rm --cached file_path
git commit -m 'delete remote somefile'
git push
```
##### git revert & reset
```js
git revert commitId //撤销某次提交，如果文件在后面的提交有修改，存在冲突可正常解决冲突
git reset commitId --hard //回到某次提交的状态，那次之后的提交丢失，包括工作区的修改，git log将不会有记录，可git reflog查看
git reset commitId --soft //回到某次提交的状态，那次之后的提交保存在暂存区，即add但未commit
git reset commitId --mixed(默认) //回到某次提交的状态，那次之后的提交保存在工作区，即未add
```

ps -ef | grep nginx 查询进程号
ps -aux|grep chromium(进程名字)

---查看端口号是否被占用-----
lsof -i:3000

---强制关闭某个已占用端口------
kill -9 13243(PID)
sudo kill -QUIT 主进程号 杀掉主进程号
