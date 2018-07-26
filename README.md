## gitbook记笔记，美滋滋～

### git book安装
```
npm install -g gitbook-cli
mkdir mybok
cd mybok
mkdir README.md SUMMARY.md //SUMMARY.md定义索引结构
gitbook init //生成索引结构文件
gitbook build //编译
gitbook serve //编译并开启server
```

### git book & github pages
```
npm install -g gh-pages
gh-pages -d _book //move dir _book from master to gh-pages
```

```
***更新***
cd mybok
gitbook init
gitbook serve
gh-pages -d _book
```