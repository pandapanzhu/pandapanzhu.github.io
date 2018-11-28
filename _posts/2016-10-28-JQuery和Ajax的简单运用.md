---
layout: post
title:  浅谈JQuery和Ajax 
date:   2016-10-28 08:32:00 +0800
categories: document
tag: 学习笔记
---

* content
{:toc}



CreateTime:2016-09-22

updateTime:2016-09-28

作为开发（不管前后台），使用到JQuery是必不可少的。

${pageContext.request.contextPath}等价于<%=request.getContextPath()%>，
表示当前项目的路径。

基础
===

选择器 
---
JQuery常用的选择器有很多种，列出几种常用的，其他可以到[菜鸟教程](http://www.runoob.com/)自主学习。

 - $("p") 		-->标签选择器
 - $("#test") 	-->id选择器
 - $(".test")	-->class选择器
 - $(":button")	-->选取type="button"或者<button>元素
 
事件
---

事情详情可以到[菜鸟教程](http://www.runoob.com/)自主学习。

捕获内容的方法
----

- .text() 	-->获取或返回元素的文本内容
- .html()	-->获取或返回元素的文本内容(包含HTML标签)
- val()	-->获取表单字段的值
- .attr()	-->获取属性值

添加元素
---

- 向前添加

	>$("#pageul").empty().preappend(html2);

先将id为“pageul”的标签中的元素清除，再在标签前面加入html2进去（html2是一系列标签）

- 向后添加

	>$("#pageul").empty().append(html2);
	
先将id为“pageul”的标签中的元素清除，再在标签后面加入html2进去（html2是一系列标签）

load方法
---

	$("#div1").load("text.txt")
将text.txt文件加入到id为“div1”的元素中去
	
	$("#div1").load("text.txt #p1")
将text.txt文件中 id为p1的标签加入到id为div1中去

off() and on()
--
on()表示通过特定的方式加载这个方法

	$("p").on("click",function(){
		alert("The paragraph was clicked.");
	});

“click” p标签之后，会有alert弹框弹出来

off()同理，点击p标签之后，移除P标签上所有的事件

	$("button").click(function(){
		$("p").off("click");
	});


获取和更改当前url
---
	
在***windows.location***中有许多关于url的标记。
	
	例子：
	window.location.href=window.location.origin+window.location.pathname
     	+"?type=chart&startDate="+$("#startDate").val()+"&endDate="+$("#endDate").val();

	windows.location.href	就是当前浏览器中的url
	windows.location.origin 就是当前的主机名+端口号
	windows.location.pathname	就是当前的url除主机名和端口号之后的后缀
	"startDate="+$("#startDate").val()	将id为startDate的值赋值给startDate

总之，url是可以通过各种手段拼接而成的。

Ajax和Json
==

get()和post()
-
- get()		-->从指点的源请求数据
- post()	-->向指定的资源提交要处理的数据

**注意：**post()也可以从服务器端获取数据，但不会缓存数据，常用于连同请求一起发送数据


data
--
当我们要上传一个表单时，我们可以先将表单序列化,将其序列化**字符串**形式，

	var data=("form").serialize();

或者，我们还可以将表单序列化成serializeArray(),转换成**JSON**格式

	var jsonData = $("form").serializeArray();
	获取数据时，就直接从json中获取，jsonData[0].name

下面给一个详细的例子：
--
	
	$(function() {
			
		$("#query_btn").click(function(){
			var condition = $("#condition").val();  //工程名字，模糊查询
			var pageSize = $("#numsize").val();		//一页显示几条数据
			QueryPro(1,pageSize,condition);			//调用QueryPro
		});
		})
	
	function  QueryPro(pageNum,pageSize,condition){
		var sendData = {
				name:condition,
				pageNum:pageNum,
				pageSize:pageSize
			}
		$.ajax({
			type:"post",
			data:sendData,
			dataType:"json",
			url:"${basePath }rest/project/QueryProject",
			//传值成功后，根据后台的responsebody接受传出来的数据
			success:function(data){
			//total总的数据条数
				$("#total").html(data.total);
				var html = "";
			//list为page当中从数据库查询到的数据，后台Page是使用github的PageInfo
				var list = data.list;
				if(list.length>0){
			//拼接td
				for(var i=0;i<list.length;i++)
					{
						html += '<tr>' + 
						'<td>' + ((data.pageNum-1)*pageSize+i+1) + '</td>' +
						'<td>' + list[i].name + '</td>' +
						'<td>' + list[i].fr + '</td>' +
						'<td>' + list[i].gldw + '</td>' +
						'<td>' + list[i].sjdw + '</td>' +
						'<td>' + list[i].jldw + '</td>' +
						'<td>' + list[i].sgdw + '</td>' +
						'<td>' + list[i].yxdw + '</td>' +
						'<td><a href="javascript:;" style="color:#6EC30B" onclick="QueryByid(this);" data-id="'+list[i].id+'">查看</a>&nbsp;|&nbsp;<a href="javascript:;" style="color:#D9534F" data-id="'+list[i].id+'">删除</a></td>'+
					'</tr>';
					}//end for
				}else{
					alert("没有搜索到您要查询的信息");
				}
				
				// 分页标签，显示上一页和下一页
				if(list.length>0){
				var html2="<li><button style='width:60px;text-align: center' id='prepage'>上一页</button>"+"</li><li id='firstPage'><button>1</button></li>";
				for(var i=1;i<data.pages;i++)
					{
					 html2+= '<li id=btn'+(i+1) +'><button>'+(i+1)+'</button></li>';
					}
				html2+='<li><button style="width:60px;text-align: center" id="nextpage">下一页</button></li>';
				}
				
				
				$("#pageul").empty().append(html2);
				$("#mytable tbody").empty().append(html);
				
				//上一页
				$("#prepage").off().click(function(){
					if (data.isFirstPage == true){
						alert("当前已是首页");
					} else {
					QueryPro(data.pageNum-1,data.pageSize,condition);	
					}
				});
				//下一页
				$("#nextpage").off().click(function(){
					if (data.isLastPage == true){
						alert("当前已是尾页");
					}else{
					QueryPro(data.pageNum+1,data.pageSize,condition);
					}
				});
				//第一页
				$("#firstPage").off().click(function(){
					if (data.isFirstPage == true){
						alert("当前已是首页");
					} else {
					QueryPro(1,data.pageSize,condition);	
					}
				});
				//第2,3,4.....页 注意闭包问题
				for(var i=1;i<data.pages;i++)
				{
					(function(arg){
						$("#btn"+(arg)).off().click(function(){
							QueryPro(arg,data.pageSize,condition);	
						});
					})(i+1);
				}
			}
		});//end ajax
	}
	

在JS中循环取出Map
===

data是从后台得到的map,在AJAX中通过success方法回调。

- 第一种方法

		var html='';//拼接html
		for( var key in data){
		html+='<option value="'+key+'">+data[key]+'</option>'
		//但当key为undefined时，这个方法就行不通了。
		}
- 第二种方法
		
		$.each(data,function(key,value){
		console.log("key:"+key+",value:"+value);
		})

	**建议使用第二种方法**

- 在js中遍历list

	for(var i,i<list.length;i++){
		value=list[i].name
	}

在JSTL中循环取出list,map,set
===

一阶Map<String,Object>
--

	<c:forEach items="${map}" var="item">
		Map的键：${item.key}
		Map的值：${item.value}
	</c:forEach>

二阶Map<String,Map<String,Object>>
--
	<c:forEach items="${map}" var="item">
		Map的键：${item.key}
		第二阶Map:<c:forEach item="${item.value}" var="items">
		第二阶Map的键：${items.key}
		第二阶Map的值：${items.value}
		</c:forEach>
	</c:forEach>

Map中包含了多个List
--
	<c:forEach item="${map.List}" var="item">
		<tr>
			<td>${item.name}</td>
			<td>${item.password}</td>
		<tr>
	<c:forEach>

遍历list与遍历一阶Map类似。
---
	<c:forEach items="${userList}" var="item">
		<tr>
			<td>${item.username}</td>
			<td>${item.username}</td>
		</tr>
	</c:forEach>

遍历Set
---
	<c:forEach item="${set}" var="item">
		<
		
js遍历JSON字符串
===

- 可以同遍历Map那样
		
		for(var i in data){
			alert(i);
			alert(data[i]);
			alert("name:"+data[i].name+",age:"+data[i].age);
		}

	或者

		for(var i = 0; i < data.length; i++){
   			alert(data[i].name + " " + data[i].password);
		}

- 如果是JSONArray,可以在外面套一层for循环

		var jsonarray =yourarray;
			for(var i =0;i<jsonarray.length;i++){
				var jsonobj = jsonarray[i];
			for(var x in jsonobj){
				document.write(x+"="+jsonobj[x]);
			}
		}

	或者

		<script type="text/javascript">  
			function text(){  
 				 var json = {"options":"[{/"text/":/"王家湾/",/"value/":/"9/"},{/"text/":/"李家湾/",/"value/":/"10/"},{/"text/":/"邵家湾/",/"value/":/"13/"}]"}   
  				json = eval(json.options)  
  				for(var i=0; i<json.length; i++)  
  				{  
     				alert(json[i].text+" " + json[i].value)  
  				}  
			}  
		</script>  

	**json = eval(json.options)**






