### 文字周边泛白

```css
{
	'text-shadow':'-1px 0 white,0 1px white,1px 0 white,0 -1px white'
}
```

### 段落开头空两格，文字两端对齐

```css
{
	text-indent: 2em;
	text-align: justify;
}
```

### 单行溢出省略，多行溢出省略

```css
//单行
overflow: hidden;
text-overflow:ellipsis;
white-space: nowrap;

//多行
//适用范围：
//因使用了WebKit的CSS扩展属性，该方法适用于WebKit浏览器及移动端；
display: -webkit-box;
-webkit-box-orient: vertical;
-webkit-line-clamp: 3;
overflow: hidden;

// 多行兼容写法
 div p{
           font-size:20px;
           font-weight: 900;
           color: crimson;
           position:relative;
           line-height:20px;
           height:60px;
           overflow:hidden;
         }
 div p::after {
           width: 20px;
           content:"...";
           font-weight:bold;
           position:absolute;
           bottom:0;
           right:-4px;
           background:antiquewhite;
           }
```

### position 四边距离为0的简写（移动端有兼容问题）

```css
.class {
  position: absolute;
  inset :0
}
```