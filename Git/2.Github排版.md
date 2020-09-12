# Github-图片排版


### 1.同一行插入多张图片，推荐在Github MarkDown用第三种方法：</br>
方法1：
```
<figure class="third">
    <img src="https://github.com/XXXXXXXXXXXX/ModernBottomNavigation/blob/master/nav1.gif" width="200">
    <img src="https://github.com/XXXXXXXXXXXX/ModernBottomNavigation/blob/master/nav3.gif" width="200">
</figure>
```
方法2：
```
<center class="half">
    <img src="https://github.com/XXXXXXXXXXXX/ModernBottomNavigation/blob/master/nav1.gif" width="300">`
    <img src="https://github.com/XXXXXXXXXXXX/ModernBottomNavigation/blob/master/nav3.gif" width="300">
</center>
```
方法3：
```
<table>
    <tr>
        <td ><center><img src="https://github.com/XXXXXXXXXXXX/ModernBottomNavigation/blob/master/nav1.gif">nav1</center></td>
        <td ><center><img src="https://github.com/XXXXXXXXXXXX/ModernBottomNavigation/blob/master/nav2.gif">nav2</center></td>
    </tr>
</table>
```
