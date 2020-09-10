# ---------------IDEA下SpringBoot项目以Jar方式部署---------------

![Image text](images/IDEA创建Jar.png)

#### 1. 在pom.xml下添加如下插件: </br>
这个插件可以在项目打包成jar包后，通过java -jar运行。如果你的pom.xml文件里面有这句话就不用添加了。
```
  <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
  </build>
```
#### 2. 在IDEA右侧Maven里面打包: </br>
在IDEA界面右侧 项目名称——>maven——>Lifecycle——>右键package——>Run Maven Build，这个菜单就和DataBase在一起，如果看不见就点击IDEA最左下角的方块图标。</br>
点击完后，IDEA下方Run窗口会有一堆信息，里面就包含着jar包的位置，如果信息太多找不到就搜索：Building jar，后面的盘符信息就是jar包的位置了。</br>
例如：Building jar: D:\IdeaProjects\why_home-master\target\back_end-0.0.1-SNAPSHOT.jar 即指明了JAR位置。</br>


#### 3. 在IDEA右侧Maven里面打包: </br>
在IDEA界面右侧 项目名称——>maven——>Lifecycle——>右键package——>Run Maven Build，这个菜单就和DataBase在一起，如果看不见就点击IDEA最左下角的方块图标。</br>
点击完后，IDEA下方Run窗口会有一堆信息，里面就包含着jar包的位置，如果信息太多找不到就搜索：Building jar，后面的盘符信息就是jar包的位置了。</br>
例如：Building jar: D:\IdeaProjects\why_home-master\target\back_end-0.0.1-SNAPSHOT.jar 即指明了JAR位置。</br>
