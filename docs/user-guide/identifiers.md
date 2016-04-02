---
---

所有Kubernetes REST API中的对象都可以通过一个Name和一个UID无歧义地辨别．

对于非唯一的，用户提供的属性，kubernetes提供了[labels](/docs/user-guide/labels)和[annotations](/docs/user-guide/annotations)．

## Names

Name通常是由客户端提供的．给定的类型的一个的对象只能一次拥有一个Name(即它们有空间的唯一性)．但是，如果你删除了一个对象，你可以给一个新对象指定该相同的Name.Name用来在Resouce的URL中指代一个对象，例如`/api/v1/pods/some-name`．传统上，Kubernetes资源的名字应该最多不超过253个字符，并且由小写的数字或字母，或者横线(`-`)和点号(`.`)组成，但是有些资源有更为特殊的限制．[identifiers design doc](https://github.com/kubernetes/kubernetes/blob/{{page.githubbranch}}/docs/design/identifiers.md)中可以查看到更多的对于命名的精确的语法规则．


## UIDs

UID是由Kubernetes生成的．每一个在Kubernetes整个生命周期钟创建的对象都有一个独有的UID（即它们在空间上并且暂时是唯一的）．
