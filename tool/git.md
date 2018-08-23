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
```
ssh-keygen -t rsa -C "your@eamil.com" //generate ssh key
cat ~/.ssh/id_rsa.pub //show ssh key
add the key to git setting
ssh -T git@github.com //testing
git config --global user.name "yournamee"
git config --global user.email "your@eamil.com"
```