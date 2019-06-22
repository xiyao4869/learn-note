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
```

ps -ef | grep nginx 查询进程号
ps -aux|grep chromium(进程名字)

---查看端口号是否被占用-----
lsof -i:3000

---强制关闭某个已占用端口------
kill -9 13243(PID)
sudo kill -QUIT 主进程号 杀掉主进程号
