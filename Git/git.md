# ---------------Git基本操作---------------

### 本地首次创建：
```
git init
git status
git add .

git commit -m "Directory structure updated"
git commit -m "Error handling page added"
git commit -m "Error handling page improved.Fragment (Thymeleaf) added,which is used to create a unified template"
git commit -m "Management of types is added"
git commit -m "Types list and query function is added in blogManage page"
git commit -m "Back_end management function is completed"
git commit -m "Home display and global query are completed"
git commit -m "Comment function is completed"


git remote rm origin
git remote add origin https://github.com/wanghaoyang949/why-home-font_end-Formatted.git
git remote add origin git@github.com:wanghaoyang949/why_home.git
//添加/更新
git push -u origin master
```


### 变换环境创建：
//需要先拉取代码
```
git pull
```
建立新文件夹 然后将代码文件覆盖
```
git pull origin master
```
若不想merge远程和本地修改，可以先创建新的分支：
```
$ git branch [name]
// 然后push
$ git push -u origin [name]
// 添加代码
git add .
// 添加注释
git commit -m ""   
// 推送代码已经关联分支
git push 
git push -u origin master
```


### 删除指定文件夹：

```
git pull origin master  //需要先拉取代码
cd            //到指定文件夹上级目录
dir          //找到要删除的文件夹名
git rm -r --cached Java_Map  //删除文件夹
git commit -m ''entity' is changed to 'po''  //提交Commit
git push -u origin master  //更新到在线仓库
```


### 官方：
```
echo "# why-home-font_end-Formatted" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/wanghaoyang949/why-home-font_end-Formatted.git
git push -u origin master
```
