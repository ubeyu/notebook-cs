# ---------------SpringBoot踩坑记录---------------

#### 关于报错：There is already 'xxxController' bean method的解决方法：</br>
报这个错的原因是因为Controller里的@RequestMapping中的路径有重复！
下面的加 @GetMapping("/loginOut")
</br></br>

#### 关于报错：Caused by: org.springframework.data.mapping.PropertyRefer enceException: No property userName found for type User!的解决方法：</br>
~~报这个错的原因是要求去写规定的代码格式，先检查一遍。
另外帮助大家排除一个坑，那就是在我这个错上，报错提示只提示字段下划线之前的内容比如create_time他报错只提示   create而_time他却直接省略了。
在自己的问题中，需要将name改为userName。错误！！！！！~~</br>

	问题在于DAO层要与其他对应！！！！！！！！！！！！！！！！！！！！！！！！！！


#### springboot中控制器的String类return "" 和 return "redirect: " 的区别：</br>
1.return "admin/blogs-afterLogin" 会自动跳转templates/下的user/blogs-afterLogin.html。</br>
2.而 return "redirect: /admin"则会返回这controller， 然后重新查找requestmapping的参数。 如果这个controller没有一个@requestmapping(value="user")，那么就会报错。</br>
因此， return+路径就是纯粹用于跳转页面。 而return+redirect就用于再次转向controller。
