---
title: example
date: 2020-10-27 08:12:43
tags: template
categories: template
mathjax: true
--- 

# 1. 分级标题  
```markdown
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
```
# 2. 分割线  

```markdown
***
---
___

```

# 3. 链接  
##行内式  
```markdown
[sunnypiggy的github](https://github.com/sunnypiggggy 'float title')
```
[sunnypiggy的github](https://github.com/sunnypiggggy 'float title')

##参考式  
github链接 [github][1], blog链接[blog][2]  

[1]:https://github.com/sunnypiggggy  
[2]:https://blog.sunnypiggy.host/
  
```markdown
github链接 [github][1], blog链接[blog][2]  

[1]:https://github.com/sunnypiggggy  
[2]:https://blog.sunnypiggy.host/  
```

# 4. 列表

- AAAA
- BBBB
- CCCC

```markdown
- AAAA
- BBBB
- CCCC
```

# 5. 引用  
```markdown
>example
>>example
>>>example
```
>example
>>example
>>>example

# 6. 图片  

```markdown

![图片alt](图片地址 ''图片title'')

图片alt就是显示在图片下面的文字，相当于对图片内容的解释。
图片title是图片的标题，当鼠标移到图片上时显示的内容。title可加可不加
```

![avatar](../uploads/avatar.png 'beautiful girl')
---
```markdown
![avatar][3]

[3]:../uploads/avatar.png
```

![avatar][3]

[3]:../uploads/avatar.png
# 7. 代码  

```python
print('shit happens!!')
```

# 8. sssxx


# 9. 注解  
引用 [^quote1] 引用 [^quote2] 引用3 [^Xe]

[^quote1]: AAAA
[^quote2]: BBBB
[^Xe]: CCCC

# 10 LateX 公式  

<http://detexify.kirelabs.org/classify.html>  

$$\begin{equation}\label{eq1}
e=mc^2
\end{equation}$$


$a = b + c$.
