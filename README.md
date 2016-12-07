# CSS-inline-block
## CSS inline-block的自动添加间距的问题和解决方法
### 搜索相关资料整理一下，以备以后查看，资料链接见最底部   
***
#### 一、现象描述
真正意义上的inline-block水平呈现的元素间，换行显示或空格分隔的情况下会有间距，很简单的个例子：  
`<input /> <input type="submit" />`
间距就来了!    
 ![Alt text](https://github.com/Ts799498164/image-folder/blob/master/inline1.png)

我们使用CSS更改非inline-block水平元素为inline-block水平，也会有该问题：   
`.space a {
    display: inline-block;
    padding: .5em 1em;
    background-color: #cad5eb;
}`    

`<div class="space">
    <a href="##">惆怅</a>
    <a href="##">淡定</a>
    <a href="##">热血</a>
</div>`     
![Alt text](https://github.com/Ts799498164/image-folder/blob/master/inline2.png)     
虽然这种表现是符合规范的应该有的表现，但是这类间距有时会对我们布局，或是兼容性处理产生影响，需要去掉它，该怎么办呢？以下展示几种常用方法，欢迎补充！     
#### 二、方法之移除空格
元素间留白间距出现的原因就是标签段之间的空格，因此，去掉HTML中的空格，自然间距就木有了。考虑到代码可读性，显然连成一行的写法是不可取的，我们可以：      
![Alt text](https://github.com/Ts799498164/image-folder/blob/master/inline3.png)    
或者是：   
![Alt text](https://github.com/Ts799498164/image-folder/blob/master/inline4.png)    
或者是借助HTML注释：   
![Alt text](https://github.com/Ts799498164/image-folder/blob/master/inline5.png)      
等。

#### 三、使用margin负值
`.space a {
    display: inline-block;
    margin-right: -3px;
}`
margin负值的大小与上下文的字体和文字大小相关，其中，间距对应大小值可以参见我之前“基于display:inline-block的列表布局”一文part 6的统计表格：
inline-block元素间间隔大小与字体和文字大小之前的关系表截图    
![Alt text](https://github.com/Ts799498164/image-folder/blob/master/inline6.png)   
例如，对于12像素大小的上下文，Arial字体的margin负值为-3像素，Tahoma和Verdana就是-4像素，而Geneva为-6像素。   
由于外部环境的不确定性，以及最后一个元素多出的父margin值等问题，这个方法不适合大规模使用。   
#### 四、让闭合标签吃胶囊
如下处理：   
![Alt text](https://github.com/Ts799498164/image-folder/blob/master/inline7.png)   
注意，为了向下兼容IE6/IE7等弱智浏览器，最后一个列表的标签的结束（闭合）标签不能丢。   
在HTML5中，我们直接：    
![Alt text](https://github.com/Ts799498164/image-folder/blob/master/inline8.png)   
好吧，虽然感觉上有点怪怪的，但是，这是OK的。   
![Alt text](https://github.com/Ts799498164/image-folder/blob/master/inline9.png)   
#### 五、使用font-size:0
类似下面的代码：
![Alt text](https://github.com/Ts799498164/image-folder/blob/master/inline10.png)   
这个方法，基本上可以解决大部分浏览器下inline-block元素之间的间距(IE7等浏览器有时候会有1像素的间距)。不过有个浏览器，就是Chrome, 其默认有最小字体大小限制，因为，考虑到兼容性，我们还需要添加：  
类似下面的代码       
![Alt text](https://github.com/Ts799498164/image-folder/blob/master/inline11.png)    
补充：根据小杜在评论中中的说法，目前Chrome浏览器已经取消了最小字体限制。因此，上面的`-webkit-text-size-adjust:none;`代码估计时日不多了。   
#### 六、使用letter-spacing   
类似下面的代码：      
![Alt text](https://github.com/Ts799498164/image-folder/blob/master/inline12.png)   
该方法可以搞定基本上所有浏览器，不过Opera浏览器下有蛋疼的问题：最小间距1像素，然后，`letter-spacing`再小就还原了。    
#### 七、使用word-spacing
类似下面代码：   
![Alt text](https://github.com/Ts799498164/image-folder/blob/master/inline13.png)   
一个是字符间距(`letter-spacing`)一个是单词间距(`word-spacing`)，大同小异。据我测试，`word-spacing`的负值只要大到一定程度，其兼容性上的差异就可以被忽略。因为，貌似，`word-spacing`即使负值很大，也不会发生重叠。     
与上面demo一样的效果，这里就不截图展示了。如果您使用Chrome浏览器，可能看到的是间距依旧存在。确实是有该问题，原因我是不清楚，不过我知道，可以添加`display: table;`或`display:inline-table;`让Chrome浏览器也变得乖巧。    
![Alt text](https://github.com/Ts799498164/image-folder/blob/master/inline14.png)   
#### 八、结语
其他去除间距的方法肯定还有，欢迎大家补充。上文部分方法可能有测试不周全之处，因此，部分细节上可能会有纰漏，欢迎指正。     
 [参考文章：Fighting the Space Between Inline Block Elements](https://css-tricks.com/fighting-the-space-between-inline-block-elements/)    [参考资料:去除inline-block元素间间距的N种方法](http://www.zhangxinxu.com/wordpress/2012/04/inline-block-space-remove-%E5%8E%BB%E9%99%A4%E9%97%B4%E8%B7%9D/)
