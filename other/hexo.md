### Basic
```
===hexo
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

{% codeblock %}
 alert('hello world')
{% endcodeblock %}

{% codeblock lang:javascript %}
 alert('hello world')
{% endcodeblock %}

Youtube-ID: I07XMi7MHd4
{% youtube I07XMi7MHd4 %}

post_asset_folder: false => post_asset_folder: true
{% asset_img cake.jpg CAKE %}
{% asset_link hexo.jpg Hexo Logo %}


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