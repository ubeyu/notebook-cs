# ---------------Java基础踩坑记录---------------

#### 2020/09/08. 关于：HashMap遍历的无序和LinkedHashMap遍历的有序：</br>
今天在博客归档页，使用如下的方式遍历HashMap里面的年份时：
```
@Override
    public Map<String, List<Blog>> archivesBlog() {
        List<String> years=blogRepository.findYearsGroup();
        Map<String,List<Blog>> archivesBlog=new HashMap<>();
        for(String year:years){
             archivesBlog.put(year,blogRepository.findBlogsGroup(year));
        }
        return archivesBlog;
    }
```
发现得到的元素不是按照之前加入HashMap的顺序输出的，原因是：HashMap散列图、Hashtable散列表是按“有利于随机查找的散列(hash)的顺序”，并非按输入顺序。遍历时只能全部输出，而没有顺序。甚至可以rehash()重新散列，来获得更利于随机存取的内部顺序。
总之，遍历HashMap或Hashtable时不要求顺序输出，即与顺序无关。
```
Map<String, List<Blog>> archivesBlog = new HashMap <String, List<Blog>>();
```
可以用java.util.LinkedHashMap 就是按加入时的顺序遍历了：
```
Map<String, List<Blog>> archivesBlog = new LinkedHashMap <String, List<Blog>>();
```
