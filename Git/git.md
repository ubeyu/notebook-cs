#---------------Git基本操作---------------

###本地首次创建：###

git init
git status
git add .


git commit -m "Directory structure updated"
git commit -m "Error handling page added"
git commit -m "Error handling page improved.Fragment (Thymeleaf) added,which is used to create a unified template"

git remote add origin https://github.com/wanghaoyang949/why-home-font_end-Formatted.git

添加/更新
git push -u origin master



###变换环境创建：###
//需要先拉取代码
git pull 
 //添加代码
git add . 
//添加注释
git commit -m ""   
//推送代码已经关联分支
git push 
git push -u origin master



###官方：###
echo "# why-home-font_end-Formatted" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/wanghaoyang949/why-home-font_end-Formatted.git
git push -u origin master
