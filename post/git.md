## Commands

```shell
git help
//clone, default master
git clone <URL>
//clone, certain branch
git clone -b <Branch name> <URL>
git status
git fetch
git pull
//Stage All Files
git add .
//Stage certain File
git add <File>
git rm --cached <File>
git commit -a -m "<Description>" 
git commit -m "<Description>" <File>
//Push commit files to remote
git push
git push origin master
//Commit history
git log
//New branch & Checkout branch
git checkout -b <Branch>
//New branch
git branch <Branch>
//Checkout branch
git checkout <Branch>
//Merge branch to current
git merge <Branch>
//Delete branch
git branch -d <Branch>
```



## Using SSH Key by Github

```shell
#1.Create SSH key
ssh-keygen -t rsa -C "your@eamil.com"
#2.Print SSH Key
cat ~/.ssh/id_rsa.pub
#3.Add SSH key to git
#4.Testing
ssh -T git@github.com
#5.Global config
git config --global user.name "yournamee"
git config --global user.email "your@eamil.com"
```



## Gitlab Pages Deploy Gitbook

1. New repo

2. Create File `.gitlab-ci.yml`

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

3. Deploy: CI/CD -> Pipelines -> Run Pipeline

4. Address：Settings -> Pages

PS: Fork repo need to remove fork relationship, Step: Settings -> General -> Advance -> remove fork relationship



## Gitlab sync to Github

1. cd to gitlab repo
2. `git remote add origin git@github.com:Jamie956/test-git.git`
3. check out github branch
4. gitlab merge to github branch


