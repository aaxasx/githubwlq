```
git init
git config --global user.email "MyEmail@gmail.com"
git config --global user.name "My Name"
git add .
git commit -m "First Commit"

git remote add origin https://github.com/
git remote add [alias] [url]
//参数[alias]为别名， [url]为远程仓库的地址

git pull github main
git push github main
git push [alias] [branch]
//[alias]为上面设置的别名，[branch]为分支名称。
//如果省略[branch]，Git会把本地的master分支推送到远程主机。

```


```
//创建仓库,在文件所在地创建
git init

//配置默认邮箱号和密码
git config --global user.name 'wlq_gitee' 
git config --global user.email '14579730+sanyunlin@user.noreply.gitee.com'

// 生成 RSA 密钥
ssh-keygen -t rsa

//获取 RSA 公钥内容，并配置到 SSH公钥 中（各种远程仓库可通用）
cat ~/.ssh/id_rsa.pub

//本地仓库链接远程仓库
git remote add gitee git@gitee.com:sanyunlin/ros2_learn.git

//保存到本地
git add .

//推送到远程仓库
git push -u gitee master

//创建.gitignore，配置忽略文件
build/
log/
install/

```
