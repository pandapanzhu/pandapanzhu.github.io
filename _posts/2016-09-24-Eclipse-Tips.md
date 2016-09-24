---
layout: post
title:  Eclipse tips
date:   2016-09-24 12:32:00 +0800
categories: document
tag: 教程
---

* content
{:toc}


作为一个Java开发人员，怎么能不知道下面这些Eclipse的小知识。


Tips
==
java智能提示 
---

- 打开Eclipse，选择打开" Window － Preferences"。
- 在目录树上选择"Java－Editor－Content Assist"，在右侧的"Auto-Activation"找到"Auto Activation triggers for java"选项。默认触发代码提示的就是"."这个符号。
- 在"Auto Activation triggers for java"选项中，将"."改成：.abcdefghijklmnopqrstuvwxyz
 

字体大小设置
----
- 打开Eclipse，选择打开" Window － Preferences"。
- 在左边的菜单栏中找到general-->appearance-->colors and fonts
- 修改*Basic*中的**Text Font**
- 点击右边的*Edit*,一般我是将字体改为11，这个依个人情况而定。

显示行号
--

- 打开Eclipse，选择打开" Window － Preferences"。
- 在左边的菜单栏中找到general-->Editors-->Text Editors
- 在右边面板中找到 *Show line number*并勾选。


背景色设置
---

- 打开Eclipse，选择打开" Window － Preferences"。
- 在左边的菜单栏中找到general-->Editors-->Text Editors

-  面板中有这样一个选项：Appearance color options；是各种板块颜色的设置，其中有一项是background color
-  选中background color，勾掉System Default，点击'color'颜色块，将出现颜色选择面板
-  根据自己的喜好选择颜色。

导出配置
---
当我们安装新的系统或使用新的电脑的时候，我们不想再从新设置我们的Eclipse，我们可以这样做。
- 点击File 选择 Export ，打开导出对话框。
- 在弹出的对话框中选择 Preference项目，然后点击Next。
- 输入一个文件路径后，就可以点击Finish保存了。
- 当我们再次想使用这个配置的时候，就可以import导入Preference项目选择对应的导出包就行了。


	
常用快捷键
==

- Ctrl+1 快速修复（最经典的快捷键,就不用多说了，可以解决很多问题，比如import类、try catch包围等）
- Ctrl+Shift+F 格式化当前代码
- Ctrl+Shift+O 组织类的import导入（既有Ctrl+Shift+M的作用，又可以帮你去除没用的导入，很有用）
- Ctrl+Y 重做（与撤销Ctrl+Z相反）
- Ctrl+D 删除当前行或者多行
- Alt+↓\↑ 当前行和下面或上面一行交互位置（特别实用,可以省去先剪切,再粘贴了）
- Ctrl+Alt+↓\↑ 复制当前行到下一行或者上一行（复制增加）
- Shift+Enter 在当前行的下一行插入空行（这时鼠标可以在当前行的任一位置,不一定是最后）
- Ctrl+/ 注释当前行,再按则取消注释
- Alt+Shift+↑ 选择封装元素
- Ctrl+←\→ 光标移到左边单词的开头或者末尾
- Ctrl+Shift+R 搜索工程中的文件
	


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
	


