---
---

# Labels

Labels是附加到对象(如Pod)的键值对。Label旨在用来为对象指定具有辨识性目的的属性，这些属性与用户相关性大，对他们来说具有意义，但是这些属性却不直接暗含核心系统的语义（sematics）。Label可以用来组织并且选择对象的子集。Label可以在对象创建的时候附加到对象上，随后可以在任意时间被添加和修改。每一个对象可以定义一组键值对Label。每一个键对于一个给定的对象必须是唯一不可重复。

```json
"labels": {
  "key1" : "value1",
  "key2" : "value2"
}
```


我们最终会对Label进行索引并且反向索引（reverse-index）labels，让查询和监视更高效，让它们可以在UI或者CLI中进行排序或者分组等等。我们不想用那些不具有辨识性效果的Label来污染Label，特别是那些体积较大和结构型数据。不具有辨识性效果的信息应该使用Annotation来记录。

## 目标

Label可以让用户将他们自己的有组织目的的结构体以一种松耦合的方式应用到系统的对象上，且不需要客户端保存这些映射关系（mappings）。

服务部署和批处理流水线(batch processing pipelines)通常是多维度的实体（例如多个分区或者部署，多个发布轨道，多层，每层多个微服务）。对它们的管理通常需要交叉剪辑(cross-cutting)的操作，这会打破原有封装好的具有严格层级的表现形式，特别对那些是由基础设施而非用户决定的,有着很死板的层级关系来说。

一些示例label:

   * `"release（发行版本）" : "stable（稳定版）"`, `"release（发行版）" : "canary（金丝雀）"`
   * `"environment" : "dev"`, `"environment" : "qa"`, `"environment" : "production"`
   * `"tier" : "frontend"`, `"tier" : "backend"`, `"tier" : "cache"`
   * `"partition" : "customerA"`, `"partition" : "customerB"`
   * `"track" : "daily"`, `"track" : "weekly"`


## 语法和字符集


Labels是键值对。合法的键值对有两个部分：一个可选的前缀和名字，由“/”分隔。名字部分是必需的，长度一定是64个字符或者更短，首位字符必须是字母数字型（`[a-z0-9A-Z]`），其他地方的字符必须是横线（`-`），下划线（`_`），点（`.`），或者其他字母数字字符。前缀是可选的。如果没有指定，前缀必须是一个dns的子域：一系列有点分隔的DNS label，总长度不能超过253个字符，以下划线“/”结尾。如果前缀被省略掉了，label的键会被认为是对于用户私有的。系统自动的组件（如kube-scheduler，kube-controller-manager，kube-apiserver，kubectl或者其他第三方自动工具）如要向用户最终的对象添加label，必须指定前缀。前缀`kubernetes.io/`是保留给Kubernetes核心组件用的。

合法的label值必须是63个或者更短的字符。要么是空，要么首位字符必须为字母数字字符，中间必须是横线，下划线，点或者数字字母。



## Label选择器

与name和UID不同，`Label`不提供唯一性。通常，我们会看到很多对象有着一样的Label。

通过Label选择器，客户端/用户能方便辨识出一组对象。Label选择器是kubernetes中核心的组织原语。

API目前支持两种选择器：基于相等性的（equality-based）选择器和基于集合的（set-based）选择器。一个Label选择器可以由多个必须条件组成，由逗号分隔。在多个必须条件指定的情况下，所有的条件都必须满足，因而逗号起着AND逻辑运算符的作用。

一个空的Label选择器（即有0个必须条件的选择器）会选择集合中的每一个对象。

 一个null型Label选择器（仅对于可选的选择器字段才可能）不会返回任何对象。


### 基于相等性的条件（_Equality-based requirement_）

基于相等性或者不相等性的条件允许用Label的键或者值进行过滤。匹配的对象必须满足所有指定的Label约束，尽管它们可能也有额外的label。有三种运算符是允许的，“=”，“==”和“!=”。前两种代表相等性（他们是同义运算符），后一种代表非相等性。例如：

```
environment = production
tier != frontend
```

第一个选择所有键名为`environment`，键值为`production`的资源。后一种选择所有键名为`tier`，键值不等于`frontend`的资源，和那些没有键名`tier`的Label的资源。

要过滤所有处于`production`但不是`frontend`的资源，可以使用逗号操作符，`environment=production,tier!=frontend`。


### 基于集合的条件（_Set-based requirement_）



基于集合的Label条件允许用一组值来过滤键名。支持三种操作符:`in`，`notin`,和`exists(仅针对于key符号)`。例如：

```
environment in (production, qa)
tier notin (frontend, backend)
partition
!partition

```

第一个例子，选择所有键名等于`environment`，且键值等于`production`或者`qa`的资源。
第二个例子，选择所有键名等于`tier`且键值是除了`frontend`和`backend`之外的资源，和那些没有键名是`tier`的label的资源。
第三个例子，选择所有所有有一个键名为partition的Label的资源；键值是什么不会被检查。
第四个例子，选择所有的没有键名为`partition`的Lable的资源；键值是什么不会被检查。

类似的，逗号操作符相当于一个__AND__操作符。因而要使用一个`partition`（不管值是什么），并且`environment`不是`qa`过滤资源可以用`partition,environment notin (qa)`。

基于集合的选择器是基于相等性的选择器的一个宽泛形式，因为`environment=production`相当于 `environment in (production)`，与`!=` and `notin`类似。

基于集合的条件可以与基于相等性 的条件混合。例如，`partition in (customerA, customerB),environment!=qa`。


## API
### LIST和WATCH过滤

LIST和WATCH操作，可以使用query参数来指定label选择器来过滤返回对象的集合。两种条件都可以使用：

 *基于相等性条件：`?labelSelector=environment%3Dproduction,tier%3Dfrontend`
 *基于集合条件的：`?labelSelector=environment+in+%28production%2Cqa%29%2Ctier+in+%28frontend%29`


两种label选择器风格都可以用来通过REST客户端来列表或者监视资源。比如使用`kubectl`来针对`apiserver`，并且使用基于相等性的条件，可以用：


```console
$ kubectl get pods -l environment=production,tier=frontend
```

或者使用基于集合的条件：

```console
$ kubectl get pods -l 'environment in (production),tier in (frontend)'
```

如以上已经提到的，基于集合的条件表达性更强。例如，他们可以实现值上的OR操作：

```console
$ kubectl get pods -l 'environment in (production, qa)'
```

或者通过exists操作符进行否定限制匹配：

```console
$ kubectl get pods -l 'environment,environment notin (frontend)'
```

### API objects中的集合指代

一些Kubernetes对象，比如service和replication controlle的，也使用label选择器来指定其他资源的集合，比如pods。

#### Service和ReplicationController

一个Service的目标Pod集合是用Label选择器来定义的。类似的，一个Replication Controller管理的Pod的群体也是用Label选择器来定义的。

对于这两种对象的Label选择器是用map定义在json或者yaml文件中的，并且只支持基于相等性的条件：



```json
"selector": {
    "component" : "redis",
}
```

或者

```yaml
selector:
    component: redis
```

这个选择器（分别是位于json或者yaml格式的）相等于`component=redis` 或者 `component in (redis)`。


### Job和其他新的资源


较新的资源，如job，也支持基于集合的条件。

```yaml
selector:
  matchLabels:
    component: redis
  matchExpressions:
    - {key: tier, operator: In, values: [cache]}
    - {key: environment, operator: NotIn, values: [dev]}
```

`matchLabels`是一个键值对的映射。一个单独的`{key,value}`相当于`matchExpressions`的一个元素，它的键字段是"key",操作符是`In`，并且值数组值包含"value"。`matchExpressions`是一个pod的选择器条件的列表。合法的操作符包含In, NotIn, Exists, and DoesNotExist。在In和NotIn的情况下，值的组必须不能为空。所有的条件，包含`matchLabels` and `matchExpressions`中的，会用AND符号连接，他们必须都被满足以完成匹配。
