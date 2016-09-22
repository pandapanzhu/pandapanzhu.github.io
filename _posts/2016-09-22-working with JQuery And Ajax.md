---
layout: post
title:  working with JQuery and Ajax
date:   2016-09-22 08:32:00 +0800
categories: document
tag: 教程
---

* content
{:toc}



作为开发（不管前后台），使用到JQuery是必不可少的。

基础
===

选择器 
---
JQuery常用的选择器有很多种，列出几种常用的，其他可以到[菜鸟教程](http://www.runoob.com/)自主学习。

 - $("p") 		-->标签选择器
 - $("#test") 	-->id选择器
 - $(".test")	-->class选择器
 - $(".button")	-->选取type="button"或者<button>元素
 
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

先将id为“pageul”的标签中的元素清楚，再在标签前面加入html2进去（html2是一系列标签）

- 向后添加

	>$("#pageul").empty().append(html2);
	
先将id为“pageul”的标签中的元素清楚，再在标签后面加入html2进去（html2是一系列标签）

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
		$("#newProject").click(function() {
			location.href = "${basePath}rest/page/newProject";
		});
		
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
	


