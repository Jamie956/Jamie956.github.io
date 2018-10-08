### Command
```
git clone git@github.com:Jamie956/test-git.git //SSH clone repo, default brench master
git clone -b dev git@github.com:Jamie956/test-git.git //SSH clone repo, from brench dev

git status //1.Current branch 2.Is up-to-date 3.Which commit

git fetch //up-to-date from remote
git fetch <url>

git pull //Pull repo
git pull <url>
git pull <url> <branch>

git commit -a -m "<balabala>" 
git commit -m "<msg>" <file>

git add <file>
git add .

git push
git push origin master

git log //List history of commit

git checkout -b <branch> //New branch & Checkout branch
git branch <branch> //New branch
git checkout <branch> //Checkout branch

git merge <branch> //合并指定branch到当前branch

git branch -d <branch> //删除branch
```

### SSH
1. 生成SSH密钥
```shell
ssh-keygen -t rsa -C "your@eamil.com"
```
2. 查看
```shell
cat ~/.ssh/id_rsa.pub
```
3. 把SSH添加到git
4. 测试
```shell
ssh -T git@github.com
```
5. 全局配置用户信息
```shell
git config --global user.name "yournamee"
git config --global user.email "your@eamil.com"
```
