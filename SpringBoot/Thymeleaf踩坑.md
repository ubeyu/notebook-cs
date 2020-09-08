# ---------------Thymeleaf踩坑记录---------------

#### 2020/08/27. 关于Thymeleaf中 th:each 的使用：</br>
在一个div中优先级最高！！ 所以放在最后的位置，前面也可以读取循环的每个值：
eg:
```
<a href="#" th:href="@{/tags/{id}(id=${tag.id})}" target="_blank" class="ui basic left pointing large label m-margin-tb-mini"  th:classappend="${tag.id == currentId} ? 'blue'" th:each="tag:${tags}">
  <span th:text="${tag.name}">前端</span>
  <div class="detail" th:text="${#arrays.length(tag.blogs)}">21</div>
</a>
```
使用1：
```
选择框的引用：
  1.  <div th:each="type:${types}" data-value="1" th:data-value="${type.id}" th:text="${type.name}">错误日志</div> 

  2.  <!--typeId 对应 data-value-->
      <div class="menu">
           <!--与后端结合 使用th:each="type:${types}"以循环的方式从types对象可以获取所有type对象 -->
           <div th:each="type:${types}" class="item" data-value="1" th:data-value="${type.id}" th:text="${type.name}">错误日志</div>
      </div>                     
 ```
 使用2：
 ```
      <!--与后端结合 使用th:each以循环的方式从page对象中的content可以获取所有tag对象 iterStat表示进行循环 -->
      <tr th:each="tag,iterStat:${page.content}">
         <!--使用th:text从iterStat中获取循环索引值-->
         <td th:text="${iterStat.count}">1</td>
         <!--使用th:text从每个type对象获取name-->
         <td th:text="${tag.name}">前端</td>
         <td data-label="操作">
             <!--返回编辑页面-->
             <!--------通过Thymeleaf中th:href---Controller---/admin/tagManage----->
             <!--使用${tag.id}从每个tag对象获取id 调用update 然后替换路径中（id）-->
             <a href="#" th:href="@{/admin/tagManage/update/{id}(id=${tag.id})}" class="ui mini teal button">编辑</a>
             <!--返回删除页面-->
             <!--------通过Thymeleaf中th:href---Controller---/admin/tagManage----->
             <!--使用${tag.id}从每个tag对象获取id 调用delete 然后删除路径中（id）-->
             <a href="#" th:href="@{/admin/tagManage/delete/{id}(id=${tag.id})}" class="ui mini red button">删除</a>
         </td>
     </tr>
```
注意1：Thymeleaf模板中每个值都用的"对象.字段"的形式，而不是调用"对象.set/get方法"的形式！！！（2020/9/4）
 ```
      <!--与后端结合 使用th:each以循环的方式从page对象中的content可以获取所有tag对象 iterStat表示进行循环 -->
      <tr th:each="tag,iterStat:${page.content}">
         <td th:text="${iterStat.count}">1</td>
         <td th:text="${tag.name}">前端</td>
         <td data-label="操作">
             <a href="#" th:href="@{/admin/tagManage/update/{id}(id=${tag.id})}" class="ui mini teal button">编辑</a>
             <a href="#" th:href="@{/admin/tagManage/delete/{id}(id=${tag.id})}" class="ui mini red button">删除</a>
         </td>
     </tr>
```

#### 2020/08/27. 关于Thymeleaf中 点击按钮触发JS脚本 的使用：</br>
 使用1：（注意id中不用#）
 ```
          // 中间部分：
         <div id="table-container">不用#
          // JS脚本：点击搜索按钮脚本
         <script>
          $('#search-button').click(function () {
            loadData();
          });
         </script>
 ```
 #### 2020/09/02. 关于Thymeleaf中 只刷新指定区域页面 的使用：</br>
 使用：（注意id中不用#）
 ```
//不能再使用form表单的submit请求，自定义loadData用于实现jQuery的ajax请求方法，通过 HTTP 请求加载远程数据，使用load方法动态获取表格内容
        function loadData() {
            //"#table-container"代表外层div，使用load方法请求地址，传递form表单内输入的title/typeId等内容
            ///*[[@{/admin/blogManage/search}]]*/为thymeleaf写法
            $("#table-container").load(/*[[@{/admin/blogManage/search}]]*/"/admin/blogManage/search",{
                title: $("[name='title']").val(),
                typeId: $("[name='typeId']").val(),
                recommendOpening: $("[name='recommendOpening']").prop('checked'),
                page: $("[name='page']").val()
            });
        }
 ```
 在其他JS脚本中引用：
 ```
        //点击搜索按钮脚本<div id="table-container">不用#
        $('#search-button').click(function () {
            //name='page'获取隐含域，val用于对其赋obj的值，obj为点击时生成对象，.data()为自定义的data-page值
            //点击搜索时，对原有状态清零
            $("[name='page']").val(0);
            loadData();
        });
        //用于点击上下页时动态处理form表单中page属性值
        function page(obj) {
            //name='page'获取隐含域，val用于对其赋obj的值，obj为点击时生成对象，.data()为自定义的data-page值
            $("[name='page']").val($(obj).data("page"));
            //点击上下页时也需要调用此方法
            loadData();
        }
 ```
 
  #### 2020/09/02. 关于Thymeleaf中 Form 表单中 th:object="${blog}" 的使用：</br>
 在form中引入：
 ```
 <!--------method="post"表单以post方式提交 对应Controller中Post----------->
 <!--------通过Thymeleaf中th:action与后台管理连接------将表单输入文章信息提交给Controller---/admin/blogManage/add----->
 <!---后端校验提示内容：th:object="${blog}"用来从后端控制器获取blog对象---对新的页面按之前值进行初始化--修改用-->
 <!---（需要放在th:object后才能获取id）th:action="*{id} == null? @{/admin/blogManage}:@{/admin/blogManage/{id}}"用来判断是[新增]还是[修改]--->
 <form id="newBlog" action="#" th:object="${blog}" th:action="*{id} == null? @{/admin/blogManage/add} : @{/admin/blogManage/update/{id}(id=*{id})}" method="post" class="ui form">
 ```
 在内部中引用对象的字段：
 ```
 <!---定义隐含域----后台传递id值--->
 <input type="hidden" name="id" th:value="*{id}">
 <!--这里是typeId 对应Blog的type的id *{type}!=null用于判断若不为空则赋值 若不加则报错！！！！！ 用于判断这是新增还是修改，若新增则没有type.ID，所有不引入，若修改则引入   -->
 <input type="hidden" name="type.id" th:value="*{type}!=null ? *{type.id}">
 ```
 
   #### 2020/09/02. 关于Thymeleaf中 th:text="|${blog.description}" 用于连接字符串 的使用：
 ```
 <!--------th:text="|${blog.description}...|"用于连接字符串------->
   <p class="m-text-article" th:text="|${blog.description}......|">
 本文将对Vue的常用系统指令进行学习，包括v-on 指令、v-text 、v-html、v-blind指令、v-model指令、v-for指令、v-if 与 v-show指令，每个指令都给出案例进行演示
   </p>
 ```
 
  #### 2020/09/03. 关于Thymeleaf中 th:text=""和th:utext="" 的使用：
   ```
 th:text="" 用于不转义的，所以输出文本中带有的<p></p>等HTML格式信息没有转义成相应格式；
 th:utext="" 用于转义的，可以恢复原始格式。
 ```
  #### 2020/09/05. 关于Thymeleaf中 th:text=""和th:utext="" 的使用：
   ```
 th:text="" 用于不转义的，所以输出文本中带有的<p></p>等HTML格式信息没有转义成相应格式；
 th:utext="" 用于转义的，可以恢复原始格式。
 ```
 
  #### 2020/09/07. 关于Thymeleaf中 th:classappend="${type.id == currentId} ? 'blue'" 的 class 判断+追加的使用：
  判断当前循环的type.id是否等于当前分类页面指定分类，若等于，则加颜色用于区分！
```
<div class="ui labeled m-margin-tb-mini button" tabindex="0" th:each=type:${types}">
   <a href="#" th:href="@{/types/{id}(id=${type.id})}" class="ui basic button" th:classappend="${type.id == currentId} ? 'blue'" th:text="${type.name}">学习日志</a>
   <a class="ui basic left pointing label" th:classappend="${type.id == currentId} ? 'blue'" th:text="${#arrays.length(type.blogs)}">24</a>
</div>
```                          
  #### 2020/09/07. 关于Thymeleaf中 org.attoparser.ParseException: (Line = 64, Column = 73) Malformed markup: Attribute "class" appears more than once in element 报错：
```
<div class="ui labeled m-margin-tb-mini button" tabindex="0" th:each=type:${types}">
  <a href="#" th:href="@{/types/{id}(id=${type.id})}" class="ui basic button" th:classappend="${type.id == currentId} ? 'blue'" th:text="${type.name}">学习日志</a>
  <!--报错：(Line = 65, Column = 26) Malformed markup: Attribute "class" appears more than once in element-->
  <a class="ui basic left pointing label" th:classappend="${type.id == currentId} ? 'blue'" th:text="${#arrays.length(type.blogs)}">24</a>
</div>
```
不认真！！！
```
th:each=type:${types}"  加前面的省略号！！！
```
  #### 2020/09/08. 关于Thymeleaf中 1.循环Map类型参数 2.th:block 循环块，可以将要循环的部分放到其内部，功能类似在外层放一个div：
```
1.用item循环接收每个Map参数：
<th:block th:each="item : ${archivesMap}">
        <!--归挡年份2017标题-->
        <h2 class="ui center aligned header">2017</h2>
        <!--归挡年份2017内容-->
        <div class="ui segments">
            <!--归档页面菜单-->
            <div class="ui fluid vertical menu ">
                <a href="#" target="_blank" class="item">
                    <span>
                        <i class="teal circle icon"></i>学习日志
                        <div class="ui teal left pointing basic label m-padded-tb-tiny">10月2日</div>
                    </span>
                    <div class="ui orange label m-padded-tb-tiny">原创</div>
                </a>
            </div>
        </div>
    </th:block>
2.th:block块的使用：
<th:block>
</th:block>
```
#### 2020/09/08. 关于Thymeleaf中 Date 日期格式化：
```
1.2020-8-13 17:30
<div class="item" th:text="|更新于 ${#dates.format(blog.updateTime, 'yyyy-MM-dd HH:mm')}|">更新于 2020-8-13 17:30</div>
2.10月2日
<div class="ui teal left pointing basic label m-padded-tb-tiny" th:text="${#dates.format(blog.updateTime, 'MMMdd')}">10月2日</div>
```
