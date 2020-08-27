# ---------------SpringBoot踩坑记录---------------

#### 2020/08/27. 关于Thymeleaf中 th:each 的使用：</br>
 使用1：</br>
```
 <div th:each="type:${types}" data-value="1" th:data-value="${type.id}" th:text="${type.name}">错误日志</div> 
 ```
 使用2：</br>
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
