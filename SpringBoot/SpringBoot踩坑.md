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
将type属性复制到type_cur</br>
```
BeanUtils.copyProperties(tag,tag_cur);
```
#### 2020/08/26. 关于SpringBoot中RedirectAttributes和Model的理解:</br>
##### RedirectAttributes
```
if(type_added == null){
	attributes.addFlashAttribute("message","新增失败！");
}else{
	attributes.addFlashAttribute("message","新增成功！");
}
return "redirect:/admin/typeManage";
```
后端校验提示内容： 使用redirectAttributes，用于@PostMapping，当Post提交完成时，添加一个返回信息，然后重定向到指定页面，页面用${#strings.isEmpty(message)}接收，并显示提示。  </br>
##### Model
```
@GetMapping("/typeManage/add") 
public String addTypePage(Model model) { 
	model.addAttribute("type", new Type());
	return "admin/types-publish";
}
```
后端校验提示内容： 使用model，用于GetMapping，当进入一个链接时增加一个新的type对象，然后将其返回到指定页面，该页面在后台存储这样一个对象，在提交时起作用。  </br>

#### 2020/08/27. IDEA连接MySQL数据库：Connection to @localhost failed. [08001] Could not create connection to database server.:</br>
路径、账户、密码都没问题，则可能JDBC碰见的时区问题，解决：</br>
在数据库路径后加：
```
jdbc:mysql://localhost:3306/why_home_database?serverTimezone=GMT
?serverTimezone=GMT
```

#### 2020/08/27. java的PO,VO,TO,BO,DAO,POJO类名包名解释:</br>
1.action包：顾名思义请求，主要是和view，即我们所说的视图就是页面打交道，action类是操作方法，对于页面Form表单的操作方法，具体操作方法的实现就在Action类里面；</br>
2.bean包：就是基本的JavaBean，多为实体；</br>
3.dao包：就是和数据库打交道的，crud即增删改查，对于数据库的增删改查的操作都在这里；
4.model包：就是实体类，就是和数据库对应，所生成表的一些属性；</br>
5.service包：服务器层，也叫业务逻辑层，调用dao中的方法，action又调用它；</br>
6.util包：即工具类，放常用到的工具方法；
7.dto、vo包：DTO = Data Transfer Object、VO = Value Object，2个概念其实是一个感念，都是用来装数据用的，而这个数据往往跟数据库没什么关系；</br>
8.O/R Mapping（ORM）是 Object Relational Mapping（对象关系映射）的缩写。通俗点讲，就是将对象与关系数据库绑定，用对象来表示关系数据。在ORM中，两个重要的概念即VO，PO。</br>VO，值对象(Value Object)，PO，持久对象(Persisent Object)，它们是由一组属性和属性的get和set方法组成。从结构上看，它们并没有什么不同的地方，但从其意义和本质上来看是完全不同的：</br>
①VO是用new关键字创建，由GC回收的。</br>
　PO则是向数据库中添加新数据时创建，删除数据库中数据时削除的。并且它只能存活在一个数据库连接中，断开连接即被销毁。</br>
②VO是值对象，精确点讲它是业务对象，是存活在业务层的，是业务逻辑使用的，它存活的目的就是为数据提供一个生存的地方。</br>
　　PO则是有状态的，每个属性代表其当前的状态。它是物理数据的对象表示。使用它，可以使我们的程序与物理数据解耦，并且可以简化对象数据与物理数据之间的转换。</br>
③VO的属性是根据当前业务的不同而不同的，也就是说，它的每一个属性都一一对应当前业务逻辑所需要的数据的名称。</br>
　　PO的属性是跟数据库表的字段一一对应的。</br>

PO对象需要实现序列化接口，是持久化对象，它只是将物理数据实体的一种对象表示，为什么需要它？因为它可以简化我们对于物理实体的了解和耦合，简单地讲，可以简化对象的数据转换为物理数据的编程。</br>
VO是值对象，准确地讲，它是业务对象，是生活在业务层的，是业务逻辑需要了解，需要使用的，再简单地讲，它是概念模型转换得到的。
</br>
PO和VO的关系应该是相互独立的，一个VO可以只是PO的部分，也可以是多个PO构成，同样也可以等同于一个PO（当然我是指他们的属性）。正因为这样，PO独立出来，数据持久层也就独立出来了，它不会受到任何业务的干涉。又正因为这样，业务逻辑层也独立开来，它不会受到数据持久层的影响，业务层关心的只是业务逻辑的处理，至于怎么存怎么读交给别人吧！不过，另外一点，如果我们没有使用数据持久层，或者说没有使用hibernate，那么PO和VO也可以是同一个东西，虽然这并不好。

#### 2020/09/02. 一直报错"Id={46"是BlogServiceImpl中update方法和Blog中tagsToIds方法的实现存在问题:</br>
```
BlogServiceImpl中update方法 错误博客修改代码：
	 @Transactional
	 @Override
	 public Blog updateBlog(Long id,Blog blog) {
		Blog blog_cur=blogRepository.findById(id).get();
		if(blog_cur == null){
		--未查到则抛出异常--
			throw new NotFoundException("该博客不存在");
		}
		/*--------对更新日期修改-----*/
		blog.setUpdateTime(new Date());
		/*--------将blog属性复制到blog_cur-----*/
		BeanUtils.copyProperties(blog,blog_cur);
		return blogRepository.save(blog_cur);
	}
BlogServiceImpl中update方法 正确博客修改代码：
	@Transactional
	@Override
	public Blog updateBlog(Blog blog) {
		/*--------对更新日期修改-----*/
		blog.setUpdateTime(new Date());
		return blogRepository.save(blog);
	}
```
```
Blog中tagsToIds方法 错误博客修改代码：
	/*---------------init()用于前端页面拿到"1,2,3..."形式的tagIds值------------*/
	    public void init(){
		this.tagIds=listToString(this.getTags());
	    }
	    /*---方法用于将list类型的tags转换为带","的String，输出给前端页面------*/
	    private String listToString(List<Tag> tags){
		StringBuffer sb=new StringBuffer();
		if(!tags.isEmpty()){
		    boolean flag=false;
		    for(int i=0;i<tags.size()-1;i++){
			sb.append(tags.get(i)+",");
		    }
		    sb.append(tags.get(tags.size()-1));
		}
		return sb.toString();
	    }
Blog中tagsToIds方法 正确博客修改代码：
	public void init() {
		this.tagIds = tagsToIds(this.getTags());
	    }
	    //1,2,3
	    private String tagsToIds(List<Tag> tags) {
		if (!tags.isEmpty()) {
		    StringBuffer ids = new StringBuffer();
		    boolean flag = false;
		    for (Tag tag : tags) {
			if (flag) {
			    ids.append(",");
		       } else {
			    flag = true;
			}
			ids.append(tag.getId());
		    }
		    return ids.toString();
		} else {
		    return tagIds;
		}
	    }    
```
#### 2020/09/02. 关于 Invalid property of bean class Bean property is not readable or has an invalid getter method 问题解决:</br>
```
org.springframework.beans.NotReadablePropertyException: Invalid property of bean class Bean property is not readable or has an invalid getter method.指向createtime.
```
检查get/set方法原因是getCreateTime()乱入了参数Date createTime.

#### 2020/09/02. 关于 new Sort(Sort.Direction.DESC,"blog.size")、new PageRequest(0,size,sort) 问题解决:</br>
```
	1./*------指定排序规则-------------------------------------*/
		旧版本：Sort sort = new Sort(Sort.Direction.DESC,"blog.size")
		新版本：Sort sort = Sort.by(Sort.Direction.DESC,"blog.size");
	2./*------取分页对象中第一页，指定size-----------------------*/
		旧版本：Pageable pageable = new PageRequest(0,size,sort)
		新版本：Pageable pageable = PageRequest.of(0,size,sort);
	3./*------调用tagRepository找到最多的前size个标签分类，此时在tagRepository定义的输入参数需要是正确的库引入的------*/
		/*----正确---*/import org.springframework.data.domain.Pageable;
		/*----错误---*/import java.awt.print.Pageable;
		return tagRepository.findTopTag(pageable);
```
#### 2020/09/03. 关于 DAO 层 @Query 查询 new Sort(Sort.Direction.DESC,"blog.size")、new PageRequest(0,size,sort) 问题解决:</br>
like ?1 代表方法内第一个参数，使用 like ?2 第二个参数时报错：
```
若用：@Query("select b from Blog b where b.title like ?2 or b.content like ?2 ")
则报错：Caused by: java.lang.IllegalArgumentException: At least 2 parameter(s) provided but only 1 parameter(s) present in query.
```
暂未找到原因。
```
正确使用：
    /*------ @Query("")自定义查询--只选择标题或正文包含字符串的文章-----*/
    @Query("select b from Blog b where b.title like ?1 or b.content like ?1 ")
    /*------Pageable根据分页对象传递参数，分页对象内有排序及大小-----*/
    Page<Blog> findSearchBlog(String query,Pageable pageable);
```
#### 2020/09/05. 天坑：！！！关于  return "redirect:/comments/" + blogId; 问题解决:</br>
"redirect:/comments/"中一定一定一定不能随便加空格！！！！！！！
```
	    /*----------博客页面评论提交逻辑（只刷新博客评论框）--------------*/
	    /* PostMapping 提交评论 */
	    @PostMapping("/comments")
	    public String addBlogsComment(Comment comment) {
		/* 获取提交评论的博客Id */
		Long blogId=comment.getBlog().getId();
		/* 将该评论设置关联博客和评论头像 */
		comment.setBlog(blogService.getBlog(blogId));
		comment.setAvatar(avatar);
		/* 保存评论 */
		commentService.saveComment(comment);
	→       return "redirect:/comments/" + blogId;
	    }
	    /*---------博客页面评论提交逻辑（只刷新博客评论框）---------------*/
```

