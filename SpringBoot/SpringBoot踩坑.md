# ---------------SpringBoot踩坑记录---------------

#### 2020/08/21. 关于报错：There is already 'xxxController' bean method的解决方法：</br>
报这个错的原因是因为Controller里的@RequestMapping中的路径有重复！</br>
下面的需要加 @GetMapping("/loginOut")</br></br>



#### 2020/08/22. 关于报错：Caused by: org.springframework.data.mapping.PropertyRefer enceException: No property userName found for type User!的解决方法：</br>
~~报这个错的原因是要求去写规定的代码格式，先检查一遍。
另外帮助大家排除一个坑，那就是在我这个错上，报错提示只提示字段下划线之前的内容比如create_time他报错只提示   create而_time他却直接省略了。
在自己的问题中，需要将name改为userName。错误！！！！！~~</br>

	问题在于DAO层要与其他对应！！！！！！！！！！！！！！！！！！！！！！！！！！</br></br>


#### 2020/08/23. 关于报错：WebMvcConfigurerAdapter is deprecated.的解决方法：</br>
WebMvcConfigurerAdapter已过时，百度上好多解决办法，但大多雷同，且有些还存在一定的误导性，可能导致最终的配置无效，不起作用.</br>
两种解决办法：</br>
// 方法一：实现WebMvcConfigurer接口</br>
public class WebConfig implements WebMvcConfigurer{</br>
    // ...</br>
}</br>

// 方法二：继承WebMvcConfigurationSupport类</br>
public class WebConfig extends WebMvcConfigurationSupport{</br>
    // ...</br>
}</br>
其实继承WebMvcConfigurationSupport之所以不起作用是因为覆盖了@EnableAutoConfiguration里的所有方法，每个方法都需要重写，比如，若不实现方法addResourceHandlers()，则会导致静态资源无法访问.
</br></br>

#### 2020/08/22. springboot中控制器的String类return "" 和 return "redirect: " 的区别：</br>
1.return "admin/blogs-afterLogin" 会自动跳转templates/下的user/blogs-afterLogin.html。</br>
2.而 return "redirect: /admin"则会返回这controller， 然后重新查找requestmapping的参数。 如果这个controller没有一个@requestmapping(value="user")，那么就会报错。</br>
因此， return+路径就是纯粹用于跳转页面。 而return+redirect就用于再次转向controller。</br></br>


#### 2020/08/23. @PostMapping("/login")注解的url，通过浏览器直接访问/admin/login会报error错误。</br></br>
#### 2020/08/23. 拦截器在访问/admin/manage或/admin/publish时返回404页面，然后又突然好了，尚未找到原因。</br></br>
#### 2020/08/24. 关于SpringDataJpa中findOne()方法报错问题:</br>
问题由来：SpringDataJPA的1.11版本，可以使用findOne()方法根据id查询。同时：</br>
findOne()：当我查询一个不存在的id数据时，返回的值是null；</br>
getOne()：当我查询一个不存在的id数据时，直接抛出异常，因为它返回的是一个引用，简单点说就是一个代理对象。</br>
所以说，如果想无论如何都有一个返回，那么就用findOne,否则使用getOne。</br>
2.x.x版本，findOne()方法报错，不能用来当作根据id查询了，</br>
需改成成findById(id).get()来查询。</br>
这是两个不同的版本，源码已经发生变化。</br></br>

#### 2020/08/26. 关于Spring中BeanUtils.copyProperties()方法:</br>
 /*--------将type属性复制到type_cur-----*/</br>
        BeanUtils.copyProperties(tag,tag_cur);</br>

#### 2020/08/26. 关于SpringBoot中RedirectAttributes和Model的理解:</br>
## RedirectAttributes
if(type_added == null){
	attributes.addFlashAttribute("message","新增失败！");
}else{
	attributes.addFlashAttribute("message","新增成功！");
}
return "redirect:/admin/typeManage";
后端校验提示内容： 使用redirectAttributes，用于@PostMapping，当Post提交完成时，添加一个返回信息，然后重定向到指定页面，页面用${#strings.isEmpty(message)}接收，并显示提示。  </br>
## Model</br>
 @GetMapping("/typeManage/add")  </br>
    public String addTypePage(Model model) {  </br>
model.addAttribute("type", new Type());</br>
return "admin/types-publish";</br>
后端校验提示内容： 使用model，用于GetMapping，当进入一个链接时增加一个新的type对象，然后将其返回到指定页面，该页面在后台存储这样一个对象，在提交时起作用。  </br>


