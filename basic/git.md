### Command
```
git clone git@github.com:Jamie956/test-git.git //SSH克隆，默认master
git clone -b dev git@github.com:Jamie956/test-git.git //SSH克隆dev分支

git status //1.Current branch 2.Is up-to-date 3.Which commit

git fetch //up-to-date from remote
git fetch <url>

git pull //拉取新的commit
git pull <url>
git pull <url> <branch>

git commit -a -m "<balabala>" 
git commit -m "<msg>" <file>

git add <file>
git add .

git push
git push origin master

git log //commit记录

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



### Gitlab Pages Deploy Gitbook

1. 新建项目

2. 创建.gitlab-ci.yml

   ```yml
   image: node:8.9
   
   cache:
     paths:
       - node_modules/
   
   before_script:
     - npm install gitbook-cli -g
     - gitbook install
   
   pages:
     stage: deploy
     script:
       - gitbook build . public
     artifacts:
       paths:
         - public
       expire_in: 1 week
     only:
       - master
   ```

3. Gitlab项目主页 -> CI/CD -> Pipelines -> Run Pipeline

4. 查看部署地址：Settings -> Pages



补充：

- fork的项目需要解除fork relationship，步骤：Settings -> General -> Advance -> remove fork relationship



### Gitlab 同步push到Github

思想：通过添加 remote url，实现 push 代码到指定url

问题：每次push都要更改（切换）地址

命令解释：

```shell
git remote //origin
git remote -v //查看关联的 remote url
git remote set-url --add origin git@github.com:Jamie956/test-git.git //添加remote url
git remote add origin git@github.com:Jamie956/test-git.git //添加remote url
git remote rm origin //移除origin
```

方法一步骤：

1. cd到gitlab项目目录下
2. `git remote -v` 查看remote url信息
3. `git remote set-url --add origin git@github.com:Jamie956/test-git.git` github添加为remote url
4. push代码，（注意，这时代码只会push到github）
5. `git remote set-url --add origin git@gitlab.com:Jamie956/test-git.git` 切换到gitlab
6. push代码

方法二步骤：

1. cd到gitlab项目目录下
2. `git remote add origin git@github.com:Jamie956/test-git.git`
3. check out github branch
4. 在gitlab branch做出更改并push代码
5. 将gitlab branch merge 到github branch



















