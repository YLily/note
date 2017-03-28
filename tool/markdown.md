# 标题
用 1-6 个井号 (#) 开始一行表示这一行是标题

* # H1
* ##  H2 
* ###  H3
* ####  H4
* #####  H5
* ######  H6

#一级标题和二级标题的另一种写法

一级标题
===
 
二级标题
---

# 分段

空一行（两个回车）分段

# 换行

行末加两个或多个空格才是真正的换行，否则正常的一个回车就像在 HTML 代码中一样，被当作空格处理 

段落和换行有什么区别？段落在生成的 HTML 代码中被一对 <p></p> 标签包含起来，而换行只是插入了一个 <br /> 标签


# Horizontal Rules 水平分割线：

***
* * *
- - -

你可以在一行中用三个以上的星号、减号、底线来建立一个分隔线，行内不能有其他东西。你也可以在星号或是减号中间插入空格

# 强调
用星号或下划线来实现。两边分别放一个 * 或 _ 会生成 em 标签，放两个则生成 strong 标签
*斜体* or _强调_
**加粗** or __加粗__
***粗斜体*** or ___粗斜体___

# Lists 列表
用星号、加号、减号开始一行表示无序列表，用数字开始一行表示有序列表

无序列表 * or + or -：

* 无序列表
* 子项
* 子项
 
+ 无序列表
+ 子项
+ 子项
 
- 无序列表
- 子项
- 子项
 
###### 有序列表：
1. 第一行
2. 第二行
3. 第三行
 
1. 第一行
- 第二行
- 第三行

# Links 连接

### 文本

1. Inline-style 内嵌方式：[link text](https://www.google.com "title text")
2. Reference-style 引用方式：[link text][id]
[id]: https://www.mozilla.org "title text"
3. Relative reference to a repository file 引用存储文件：[link text](../path/file/readme.text "title text")

### Images 图片：

1. Inline-style 内嵌方式： 
![alt text](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "title text")
 
2. Reference-style 引用方式： 
![alt text][logo]
[logo]: https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "title text"

# code and Syntax Highlighting 代码和语法高亮：

如果是在一段文字中插入一句代码，把代码用 (`) 符号包围起来即可

`hightlight`
`` `包含此符号` ``

Block Code 代码分组(代码区块)：

    在该行开头缩进4个空格或一个制表符(tab)

语法高亮：
```html
    <div>Syntax Highlighting</div>
```
```css
    body{font-size:12px}
```
 
```javascript
    var s = "JavaScript syntax highlighting";
    alert(s);
```
```php
    <?php
      echo "hello, world!";
    ?>
```
```python
    s = "Python syntax highlighting"
    print s
```

# Blockquotes 引用：
右尖括号 (>) 表示 blockquote，可以嵌套，可以包含其它的 Markdown 元素
> Email-style angle brackets
> are used for blockquotes.
> > And, they can be nested.
> #### Headers in blockquotes
> * You can quote a list.
> * Etc.

# 表格
```
| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |
```

这种语法生成的表格如下：

| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |


# Escape character 转义符(反斜杠)：

\*literal asterisks\*
Markdown 支持以下这些符号前面加上反斜杠来帮助插入普通的符号：
\反斜杠  `反引号  *星号  _下划线  {}花括号  []方括号  ()括弧  #井字号  +加号  -减号  .英文句 !感叹号

# MathJax公式

支持[MathJax公式](http://www.mathjax.org/)

 `$ a^2 + b^2 = c^2 $`
```mathjax
x = \dfrac{-b \pm \sqrt{b^2 - 4ac}}{2a}
```