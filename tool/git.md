### Command
```
git status

git clone <url>
git fetch <url>
git pull <url> <branch>

git commit -m "<msg>" -a
git commit -m "<msg>" <file>

git add <file>
git add .

git push
git push origin master
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
