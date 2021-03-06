# Vim

## 索引配置

​	在Rest风格设置 /_settings 或者 {index}/_ _settintgs 设置 一个或者多个索引

```http
PUT http://127.0.0.1:9200/secisland/_settings

{
	“index”：{“number_of_replicas”:4}
}
```

​	在更新分词器，创建索引后添加新的分析器，添加分析器之前必须关闭索引，添加之后关闭

```
POST http://127.0.0.1:9200/secisland/_close
PUT http://127.0.0.1:9200/secisland/_settings
{
	“analysis”{
		“analyzer”{
			“content”：{“type”:"custom","tokenizer":"whitespace"}
		}
	}
}
POST http://127.0.0.1:9200/secisland/_open
```

获取配置 索引中包含很多参数。可以通过命令获取索引的参数配置

```
GET http://127.0.0.1:9200/secisland/_settings
host:port/{index}/_settings
GET http://127.0.0.1:9200/secisland/_settings/name=index.number_*
```

这是拿索引配置

## 索引分析

​	把文本块分析成一个个单独的词(term)，为了后面的倒排做准备，然后标准化为标准形式。提高可搜索性。 都是分析器完成的，分析器有三个功能

- 字符过滤器 字符串经过（character filter）处理，标记化 之前处理字符，能够去出HTML标记，或者转化为“&”或 and
- 分词器：分词器被标记化成独立的词，一个简单的分词器可以根据空格或者逗号把单词分开
- 标记过滤器：每个词都通过所有标记过滤（token filters）处理，它可以修改词 ，例如将“Quick”转为小写怕0



## 映射

映射是定义存储和索引的文档类型以及字段的过程，索引中的每一个文档都有 一个类型。 每种类型都有自己的映射。



### 概念

- 每个索引有一个或者多个映射类型，用于将索引中文档划分成不同的逻辑组
  - 元字段 用于定义如何处理文档的元数据， 元字段包含文档的 _index 字段， _type 字段 _id 字段，和 _ source 字段
  - 字段或属性：每个映射类型包含与类型相关的字段或者属性列表。同一个索引不同映射类型的相同名字字段必须有相同的映射
- 字段拥有一个数据类型，可以是简单的数据类型，比如字符串类型，日期型，长整型，双精度浮点，布尔型，或者IP，。 支持JSON层次类型

### 动态映射

字段和映射类型在使用前不需要事先定义，依靠动态映射，通过索引文档，新的映射类型和字段名会自动添加，新的字段可以添加到顶级映射类型或者映射内部的对象和嵌入字段，动态映射可以配置自定义映射用于新类型或者新字段

### 显示映射

对于es 可以尝试使用显示映射，而不是动态映射，使用显示

### 更新当前映射

除了记录之外，现有的映射类型和字段不能更新，修改映射以为着废弃已经索引的文档，我们反而应该更新索引，

### 映射之间共享字段

映射类型在每个索引中是唯一的，就是在一个索引中的多个类型中，如果多个类型的映射名称一样，必须是相同的类型

例如： title 字段哦同时存在于user 和 blogpost映射类型，就是在一个索引的多个类型中，如果多个类型映射名字一样，他们必须是相同的类型



### 字段数据类型

- string 字符类型，已经被废弃了，现在是text
- 数字类型 long int short byte，short byte double float
- 日期型数据类型：date
- 布尔型数据类型： boolean
- 二进制数据类型 binary

复杂数据

- 数组数据类型：不需要专门的类型定义数组

- geo_point 经纬点
- object 单独的json 对象的数组
- geo_shape 多边形复杂地理形状



IPV4数据类型，IP协议为IPv4地址

完成数据类型：completion 提供自动补全的建议

