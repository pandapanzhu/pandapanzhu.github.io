---
layout: post
title:  浅谈我所知的MongoDB
date:   2016-09-21 08:32:00 +0800
categories: document
tag: 教程
---

* content
{:toc}



MongoDB是目前比较流行的一种非关系型数据库（NoSql），他的优势很多，这里不废话。


入门
===

下载 
---

- 使用MongoDB首先需要到[官网](https://www.mongodb.org/downloads#production)或者我的[百度网盘](http://pan.baidu.com/s/1hsrzqM4)去下载。
安装
---

- 下载完成后直接安装就好，我们可以在安装的时候选择*customer*的安装方式选择自己想要安装的路径。

建立一个存放数据库的文件夹
----

- 一般我会在MongoDB的安装根目录下创建一个名为*db*的文件夹，这个没有固定要求。

启动MongoDB
---

- 在Windows下打开命令行DOS窗口（win+R），首先进入MongoDB安装目录下的bin目录下，也可以在环境变量中加入MongoDB安装的目录下的bin文件夹路径，然后用**mongod --dbpath**来启动。

基本操作
===

增删查改是一个数据库最基本的操作，我们可以打开一个DOS命令窗口（启动窗口不要关闭），输入***mongo***就能打开MongoDB的shell命令客户端。

1. 增

	>db.Person.insert({"name":"张三","role":"admin"});
	WriteResult({ "nInserted" : 1 })
	
		
2. 查

	条件查询：

	>db.Person.find({"name":"张三"});
	{ "_id" : ObjectId("5617275737a5aa2cafdb4b84"), "name" : "张三", "role" : "admin" }
	>
	全部查询：

	>db.Person.find();
	{ "_id" : ObjectId("5617275737a5aa2cafdb4b84"), "name" : "张三", "role" : "admin" }

	**注意：**id字段是默认加入的GUID，保证了数据的唯一性。就像关系型数据库中的***主键***，在MongoDB中就省去了定义主键这一步了。
3. 改

	update命令有两个参数，一个是“查找条件”，另一个是“更新的值”，其大概可以看成是一个**Map**的Key和Value。

	>db.Person.update({"name":"张三"}，{"name":"张三","age":40});
	WriteResult({ "nMatched" : 1, "nUpserted" : 0, "nModified" : 1 })

	>db.Person.find();
	{ "_id" : ObjectId("5617275737a5aa2cafdb4b84"), "name" : "张三", "role" : "admin","age":40}

4. 删

	>db.Person.delete({"name":"张三"});
	WriteResult({ "nRemoved" : 1 })

	>db.Person.find();
	
	就没有数据了。

将MongoDB安装到window服务里
===

1. 创建一个存放数据库日志的文件夹，比如“D:\Program Files\MongoDB\Server\3.0\log”。
2. 在根目录下创建一个名为“mongod.cfg”配置文件，如下：
	systemLog:
    destination: file
    path: D:\Program Files\MongoDB\Server\3.0\log\mongod.log
	storage:
    dbPath: D:\Program Files\MongoDB\Server\3.0\db
	
	具体的配置参数，请参照[官方文档](http://docs.mongodb.org/manual/reference/configuration-options/)
	
	**注意：**配置文件内容不能用“缩进符号”来对齐，在目前我使用的版本3.0.6会无法启动。
		
3. 以**管理员权限**打开一个“命令指示符”窗口，使用如下命令：
	D:\Program Files\MongoDB\Server\3.0\bin>mongod --config "D:\Program Files\MongoD

	B\Server\3.0\mongod.cfg" --install

	D:\Program Files\MongoDB\Server\3.0\bin>
4. 在DOS命令窗口输入：net start MongoDB 即可。

在Mybatis中使用MongoDB操作
===

MyBatis增操作
--

**注意：** 在Mybatis增加操作时，要先将数据存放在对应的class中，再将这个class存放在MongoDB中，这样才能在以后的操作中使用对应的操作语句。
	
	   public void addData() {  
        DBCollection coll = MongoUtil.getColl("wujintao");  
        BasicDBObject doc = new BasicDBObject();  
        doc.put("name", "MongoDB");  
        doc.put("type", "database");  
        doc.put("count", 1);  
  
        coll.insert(doc);  
		//插入时，需要将数据加到doc中才能在MongoDB中找到对应的class
        // 设定write concern，以便操作失败时得到提示  
        coll.setWriteConcern(WriteConcern.SAFE);  
    }  
    

Mybatis查操作
--
	private MongoOperations mongo;

	
	DBObject query =new BasicDBObject();
	query.put("createdTime",new BasicDBObject(QueryOperators.GTE, beginTime).append(QueryOperators.LTE, endTime));
	//其中GTE代表>=,LET代表<=
	query.put("url",url);
	//查询是可以拼接的，相当于MySQL中的AND
	
	
	//模糊查询,模糊查询是根据java正则表达式来匹配的
	Pattern pattern=Pattern.compile(url);//url从后台传过来的
	query.put("url",url);
	
	
	long count=mongo.getCollection(COLLECTION).count(query);
	//得到上述query查询到的数据的数量

	DBCursor cursor = mongo.getCollection("wujintao").sort(new BasicDBObject("Id", -1)).find(query).skip(page.getStartItem()).limit(page.getPageSize()); 
	//分页,并根据id顺序显示
	
	
如果只想查询数据表中一部分数据，可以定义一个buildFields函数，将需要查询的数据写出来，相当于MySQL中的SELECT id,name,,role from ***;
	public static DBObject buildFields(String... fieldNames) {
        DBObject fields = new BasicDBObject();
        for (String fn : fieldNames) {
            fields.put(fn, 1);
        }
        return fields;
    }

	//调用上述函数
	 DBObject fields =buildFields("id", "name", "role");
	
	DBCursor cursor = mongo.getCollection(COLLECTION).find(query, fields).sort(new BasicDBObject("id", -1)).skip(page.getStartItem()).limit(page.getPageSize());

最后将查询到数据放到List中

	List<RepeatArticle> result = new LinkedList<RepeatArticle>();
        while (cursor.hasNext()) {
            DBObject o = cursor.next();
            result.add(toEntity(o, RepeatArticle.class));
        }
        return result;
	
	
Mybatis删操作
---

	public void delete() {  
        BasicDBObject query = new BasicDBObject();  
        query.put("name", "xxx");  
        // 找到并且删除，并返回删除的对象  
        DBObject removeObj = mongo.getCollection("wujintao").findAndRemove(query);  
        System.out.println(removeObj);  
    }  

Mybatis改操作
---

	public void update() {  
        BasicDBObject query = new BasicDBObject();  
        query.put("name", "liu");  
        DBObject stuFound = mongo.getCollection("wujintao").findOne(query);  
        stuFound.put("name", stuFound.get("name") + "update_1");  
		//stuFound.get("name") + "update_1" 为修改后的name
        mongo.getCollection("wujintao").update(query, stuFound);  
    }  

至此，以后有深入了解在回来补充！
参考文献

- [MongoDB基本用法](http://javacrazyer.iteye.com/blog/1840042)
- [MongoDB——基础入门](http://www.cnblogs.com/sheepswallow/p/4863578.html)




