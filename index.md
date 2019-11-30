# 简介

不知道该写点啥，该博客暂定写一些文章，例如程序设计语言，编程的一些方法总结，数据结构与算法，操作系统之类的，另外可能还有一些强化学习的内容，云计算之类的。



# 语言

[c](files/c-language.md)

[c++]()

[python](files/python.md)



[test1](test/latex-formula.md)

[test2](test/test.md)

[test3](test/xtest.html)

# 论文翻译

[Resource-Management-with-Deep-Reinforcement-Learning](files/Resource-Management-with-Deep-Reinforcement-Learning/Resource-Management-with-Deep-Reinforcement-Learning.html)

[adaptive-dispatching-of-tasks-in-the-cloud](files/adaptive-dispatching-of-tasks-in-the-cloud.md)

[Resource-Management-with-Deep-Reinforcement-Learning](files/Resource-Management-with-Deep-Reinforcement-Learning)

# 网页问题

目前基本解决公式显示问题（主要需要与typora同步，希望两个平台上都正确显示）

mathjax3在模板文件的`<head>`标签中添加以下代码

```html
<script>
MathJax = {
  tex: {
    inlineMath: [['$', '$']],
    macros: {
      infin: "{\\infty}",
      empty: "{\\emptyset}"
    }
  },
    options: {
    renderActions: {
      /* add a new named action not to override the original 'find' action */
      find_script_mathtex: [10, function (doc) {
        for (const node of document.querySelectorAll('script[type^="math/tex"]')) {
          const display = !!node.type.match(/; *mode=display/);
          const math = new doc.options.MathItem(node.textContent, doc.inputJax[0], display);
          const text = document.createTextNode('');
          node.parentNode.replaceChild(text, node);
          math.start = {node: text, delim: '', n: 0};
          math.end = {node: text, delim: '', n: 0};
          doc.math.push(math);
        }
      }, '']
    }
  }
};
</script>

<script type="text/javascript" id="MathJax-script" async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js">
</script>
```

由于目前mathjax3不支持texvc，所以暂时添加了两个用到的宏

## 几个问题

### 使用`$...$`出现的问题

#### 转义问题

因为kramdown解析markdown时，会将`\\`转义成`\`，所以行内公式（使用一对`$`）的换行显示将会出现问题，一般在矩阵中，可以使用`$$...$$`的方式。

更确切地说，除非`\`后面跟的是字母，如果是符号可能会当成转义，即，如果`\`后面跟的是符号，使用`$$...$$`



由于markdown对`_`对会解析成斜体，例如 ` _123_ `，如果左右`_`正好左边右边都没有字母，那么会解析成斜体，例如`\mathbf{r}_1=x_{1,2}`，此时kramdown会将由`$`包裹的`_`解析成`<em>`标签，解决方法有避免出现这种情况，如果出现使用`$$...$$`方式

#### 行间公式

行间公式注意`$$`前后各需要一个换行，否则可能会识别成行内公式从而左对齐