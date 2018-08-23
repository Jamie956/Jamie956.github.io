## Gitbook 记笔记，美滋滋～

### 初始化

| 命令                       | 说明              |
| -------------------------- | ----------------- |
| npm i -g gitbook-cli       | 全局安装 gitbook  |
| mkdir mybok                | 创建 book 文件夹  |
| cd mybok                   |                   |
| touch README.md SUMMARY.md | 创建索引结构      |
| gitbook init               | 生成索引结构文件  |
| gitbook build              | 编译              |
| gitbook serve              | 编译并开启 server |

### 使用 gh-pages 提交

| 命令               | 说明                                     |
| ------------------ | ---------------------------------------- |
| npm i -g gh-pages  | 全局安装 gh-pages                        |
| gh-pages -d \_book | 将文件夹\_book 的内容同步到分支 gh-pages |

###　更新流程

| 命令               | 说明                |
| ------------------ | ------------------- |
| cd mybok           | 到笔记路径下        |
| gitbook init       | 生成目录结构        |
| gitbook serve      | 开始 serve 调试     |
| gh-pages -d \_book | 发布到分支 gh-pages |
