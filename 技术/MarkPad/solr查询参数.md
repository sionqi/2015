## Solr 查询参数说明
 
 
## 常用参数
**q** - 查询字符串，这个是必须的。如果查询所有`*:*` ，根据指定字段查询
```
Name:张三 AND Address:北京
```

**fq** - （filter query）过虑查询，作用：在q查询符合结果中同时是fq查询符合的，例如：
```
q=Name:张三&fq=CreateDate:[20081001 TO 20091031],找关键字mm，并且CreateDate是20081001
```

**fl** - 指定返回那些字段内容，用逗号或空格分隔多个。

**start** - 返回第一条记录在完整找到结果中的偏移位置，0开始，一般分页用。

**rows** - 指定返回结果最多有多少条记录，配合start来实现分页。

**sort** - 排序，格式：`sort=<field name>+<desc|asc>[,<field name>+<desc|asc>]`
```
score desc, price asc表示先 “score” 降序, 再 “price” 升序，默认是相关性降序。
```

**wt** - (writer type)指定输出格式，可以有 xml, json, php, phps。

fl表示索引显示那些field( *表示所有field,如果想查询指定字段用逗号或空格隔开（如：Name,SKU,ShortDescription或Name SKU ShortDescription【注：字段是严格区分大小写的】）)

**q.op** 表示q 中 查询语句的 各条件的逻辑操作 AND(与) OR(或)

**hl** 是否高亮 ,如hl=true

**hl.fl** 高亮field ,hl.fl=Name,SKU

**hl.snippets** :默认是1,这里设置为3个片段

**hl.simple.pre** 高亮前面的格式

**hl.simple.post** 高亮后面的格式

**facet** 是否启动统计
**facet.field**  统计field

【注：以上是比较常用的参数，当然具体的参数使用还是多看Solr官方的技术文档以及一些大神的博文日志，这里只是抛砖引玉】
 
二、 Solr运算符
1. “:” 指定字段查指定值，如返回所有值*:*
1. “?” 表示单个任意字符的通配
1.  “*” 表示多个任意字符的通配（不能在检索的项开始使用*或者?符号）
1.  “~” 表示模糊检索，如检索拼写类似于”roam”的项这样写：roam~将找到形如foam和roams的单词；roam~0.8，检索返回相似度在0.8以上的记录。
1. 邻近检索，如检索相隔10个单词的”apache”和”jakarta”，”jakarta apache”~10
1. “^” 控制相关度检索，如检索jakarta apache，同时希望去让”jakarta”的相关度更加好，那么在其后加上”^”符号和增量值，即jakarta^4 apache
1. 布尔操作符AND、||
1. 布尔操作符OR、&&
1. 布尔操作符NOT、!、- （排除操作符不能单独与项使用构成查询）
1. “+” 存在操作符，要求符号”+”后的项必须在文档相应的域中存在
1. ( ) 用于构成子查询
1. [] 包含范围检索，如检索某时间段记录，包含头尾，date:[200707 TO 200710]
1. {} 不包含范围检索，如检索某时间段记录，不包含头尾 date:{200707 TO 200710}
1. / 转义操作符，特殊字符包括+ - && || ! ( ) { } [ ] ^ ” ~ * ? : /

 注：①“+”和”-“表示对单个查询单元的修饰，and 、or 、 not 是对两个查询单元是否做交集或者做差集还是取反的操作的符号
　　 比如:AB:china +AB:america ,表示的是AB:china忽略不计可有可无，必须满足第二个条件才是对的,而不是你所认为的必须满足这两个搜索条件
　　 如果输入:AB:china AND AB:america ,解析出来的结果是两个条件同时满足，即+AB:china AND +AB:america或+AB:china +AB:america
　　总而言之，查询语法：  修饰符 字段名:查询关键词 AND/OR/NOT 修饰符 字段名:查询关键词
三、 Solr查询语法
1.最普通的查询，比如查询姓张的人（ Name:张）,如果是精准性搜索相当于SQL SERVER中的LIKE搜索这需要带引号（""）,比如查询含有北京的（Address:"北京"）
2.多条件查询，注：如果是针对单个字段进行搜索的可以用（Name:搜索条件加运算符(OR、AND、NOT) Name：搜索条件）,比如模糊查询（ Name:张 OR Name:李 ）单个字段多条件搜索不建议这样写，一般建议是在单个字段里进行条件筛选，如（ Name:张 OR 李），多个字段查询（Name:张 + Address:北京 ）
3.排序，比如根据姓名升序（Name asc）,降序（Name desc）