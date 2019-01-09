**Commands**


| -                      |               |
| -------------------------- | ----------------- |
| npm i gitbook-cli       | 安装 gitbook  |
| touch README.md SUMMARY.md | 创建索引结构      |
| gitbook init               | 生成索引结构文件  |
| gitbook build              | 编译              |
| gitbook serve              | 编译并开启 server |
| npm i gh-pages | 安装 gh-pages |
| gh-pages -d _book | 将文件夹_book 的内容同步到分支 gh-pages |
| gitbook install      | 安装gitbook插件 |



**Quickly Start**

```js
npm i
npm run ins
npm run ini
npm run serve
npm run sync
```



**Install Plugings**

1. touch book.json
```
{
  "plugins": ["page-treeview"]
}
```

2. install
```
gitbook install
```



**book.json config**

```json
{
  "plugins": ["page-treeview","splitter"],
  "title" : "Jamie House",
  "author" : "Jamie",
  "description" : "V5"
}
```

