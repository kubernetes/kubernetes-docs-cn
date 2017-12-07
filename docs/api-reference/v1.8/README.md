---
title: README
cn-approvers:
- WinSon-XiaoYin
---

<!-- 
## Synopsis

Static compilation of html from markdown including processing for grouping code snippets into arbitrary tabs.
-->

## 概要

从markdown编译静态html，包括将代码片段分组到任意选项卡中。

<!--
## Code Example

\> bdocs-tab:kubectl Deployment Config to run 3 nginx instances (max rollback set to 10 revisions).

bdocs-tab:tab will be stripped during rendering and utilized to with CSS to show or hide the prefered tab. kubectl indicates the desired tab, since blockquotes have no specific syntax highlighting.

\`\`\`bdocs-tab:kubectl_yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: deployment-example
spec:
  replicas: 3
  revisionHistoryLimit: 10
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.10
\`\`\`

bdocs-tab:tab_lang will be used to indicate which tab these code snippets belong to. The tab section of the string indicates the tab, while, the language is pushed beyond the underscore. During rendering, the language will be properly highlighted as if the bdoc token was omitted.
-->

## 代码示例

\> bdocs-tab:kubectl Deployment Config 启动3个nginx实例(最大回滚设置为10次)。
bdocs-tab:在渲染过程中，标签将被剥离，并使用CSS来显示或隐藏优先选项卡。通过kubectl指示所需的选项卡，因为blockquotes没有语法高亮显示。

\`\`\`bdocs-tab:kubectl_yaml
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: deployment-example
spec:
  replicas: 3
  revisionHistoryLimit: 10
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.10
\`\`\`

bdocs-tab:tab_lang将用于指示这些代码片段属于哪个选项卡。字符串的选项卡部分表示选项卡，而语言则被推到下划线之外。
在呈现过程中，语言将被适当地突出显示，就像bdoc令牌被省略了一样。


<!--
## Motivation

This is a project to extend markdown documents and render them in html with a table of contents and code snippet pane. Most projects of this variety lean heavily on front end parsing with JavaScript/jQuery. This project uses NodeJS, Marked, and highlight.js to output syntax highlighted code blocks.

With specific tokens on blockquotes and code blocks, the chunks can be placed according to their relevance. Ex: Multiple language code blocks that should be grouped under an arbitrary tab.
-->

## 动机

这是一个扩展markdown文档和在html中使用内容表和代码片段窗格来呈现它们的项目。大多数这一类的项目主要依靠JavaScript/jQuery来进行前端解析。这个项目使用NodeJS，Marked和highlight.js来输出语法高亮代码块。

<!--
## Installation

Clone the repository, then add documents into documents directory. Modify the manifest.json to contain the document filenames in the order desired. The docs field is an array of objects with a filename key.

As a NodeJS program, a valid installation of node is required. Once node is installed, verify it can be run from command line.
```
node --version
```
Next, depedencies need to be installed via npm from the root of the project directory.
```
npm install
```

Once dependencies are installed, run
```
node brodoc.js
```

This will generate the index.html file, which can be opened in a browser or served.

The included node-static server can be run from the project root via
```
npm start
```
-->

## 安装

克隆仓库，然后将文档添加到文档目录中。修改manifest.json包含希望排序的文档的文件名。

作为一个NodeJS程序，需要安装一个能用node。安装完node之后，在命令行运行下面的命令验证是否可用。
```
node --version
```

接下来在项目的根目录下通过npm安装所需的依赖。
```
npm install
```

依赖安装完成之后，执行下面的命令
```
node brodoc.js
```

执行完上一条命令将生成一个index.html文件，可以在浏览器中打开它。

node-static服务器可以在项目的根目录执行下面的命令启动
```
npm start
```


<!--
## License

Apache License Version 2.0
-->

## 许可

Apache License Version 2.0


<!--
## FAQ

Q: Why is it named brodocs?
A: This project was born out of a collaboration with my brother to create a suitable docs app for his purposes. It was a fun name for the the two of us to use as actual brothers.
-->

## 疑难问题

Q：为什么叫做brodocs？
A：这个项目是与我的兄弟合作创建一个满足他要求的文档应用程序。对我们两人来说，这是一个有趣的名字，他们是真正的兄弟。