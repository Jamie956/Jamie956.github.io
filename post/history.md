## windows

**Service start/stop/delete**

```
sc delete <service>
net stop <service>
net start <service>
```

**查看 port**

```shell
netstat -ano | findstr <port>
tasklist|findstr <PID>
```

**查看win10系统激活情况**

```
slmgr.vbs -xpr
```



## VSCode

**Settings**

```json
{
    "editor.fontSize": 12,
    "explorer.confirmDelete": false,
    "workbench.iconTheme": "vscode-icons",
    "explorer.confirmDragAndDrop": false,
    "editor.tabSize": 2,
    "files.associations": {
        "*.ejs": "html"
    },
    "open-in-browser.default": "chrome"
}
```



**快捷键**

- ctrl+k+0 //折叠代码
- ctrl+k+j //打开代码
- shift+alt+i //编辑多行行尾
- shift+alt+选中 //多行编辑



**plugin**

- Local History
- open in browser
- Prettier
- vscode-icons



## Typora

ctrl + 1 //标题1

ctrl + 2 //标题2

...

ctrl + 5 //标题5

ctrl + u //下划线

ctrl + b //加粗

ctrl + i //斜体

ctrl + k //超链接 [百度](http://www.baidu.com)

ctrl + t //表格

\> + 空格 //引用

ctrl + l //选中行

ctrl + d //选中词

alt + shift + 5 //删除线

ctrl + f //搜索

ctrl + h //搜索替换

ctrl + shift + i //插入图片

\- + 空格 //无序列表

1 + . //有序列表



## Eclipse
**快捷键**

- ctrl+alt+down //复制并粘贴一行代码

- ctrl+shift + /(数字小键盘) //折叠代码

- ctrl+shift + *(数字小键盘) //展开代码



**代码提示**

preferences -> java -> editor -> content assist -> auto activation triggers for java -> 输入abcdefghijklmnopqrstuvwxyz. 



## Hexo

```shell
npm install -g hexo-cli
hexo -v
hexo init
npm i
hexo server

hexo new a
hexo new draft b
hexo server --draft

hexo publish b

hexo new page c

default_layout

[tag1, tag2]

categories:
- [cat1, cat1.1]
- [cat2]
- [cat3]

ssh-keygen -t rsa -C "752492509@qq.com"
C:\Users\bxm09\.ssh\id_rsa.pub
https://github.com/settings/ssh => new SSH key
ssh -T git@github.com
git config --global user.name "jamie956"
git config --global user.email "752492509@qq.com"

npm install hexo-deployer-git --save

hexo clean
hexo g
hexo d

[Writing](https://hexo.io/docs/writing.html)
```

