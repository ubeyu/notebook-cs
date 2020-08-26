# ---------------Git基本操作---------------

### 本地首次创建：

git init</br>
git status</br>
git add .</br>


git commit -m "Directory structure updated"</br>
git commit -m "Error handling page added"</br>
git commit -m "Error handling page improved.Fragment (Thymeleaf) added,which is used to create a unified template"</br>
git commit -m "Management of types is added"

git remote add origin https://github.com/wanghaoyang949/why-home-font_end-Formatted.git</br>
git remote add origin git@github.com:wanghaoyang949/why_home.git</br>
添加/更新</br>
git push -u origin master
</br>



### 变换环境创建：
//需要先拉取代码</br>
git pull </br>
建立新文件夹 然后将代码文件覆盖</br>
git pull origin master</br>

若不想merge远程和本地修改，可以先创建新的分支：</br>
$ git branch [name]</br>
然后push</br>
$ git push -u origin [name]</br>
</br>

 //添加代码</br>
git add .
</br>
//添加注释</br>
git commit -m ""   
</br>
//推送代码已经关联分支</br>
git push 
</br>
git push -u origin master
</br>



### 官方：
echo "# why-home-font_end-Formatted" >> README.md
</br>
git init
</br>
git add README.md
</br>
git commit -m "first commit"
</br>
git remote add origin https://github.com/wanghaoyang949/why-home-font_end-Formatted.git
</br>
git push -u origin master
</br>
