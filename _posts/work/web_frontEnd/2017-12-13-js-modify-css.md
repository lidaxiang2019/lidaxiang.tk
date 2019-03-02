---
layout: post
title:  "JS 改 Css 和 Style"
rootCate: "work"
categories:
- Web_FrontEnd
tags:
- work
- web_frontEnd  
- JavaScript
- Experience
---

JS 和 CSS 经验总结，整理一些常见的坑以备后用。

<!---more--->
> StartTime: 2017-12-13,ModifyTime: 2017-12-13

1.用js改css和style：

```
	var headTop = document.getElementById("headTop");
	// headTop.style.position = "fixed";
	    headTop.style.padding-Left = "40px";
	    headTop.style.width = "1150px";
```
2.用jquery改css：
```
	$("#searchDiv").css("width","1150px");
```

3.// 获取radio的value值方法一(jquery)
```
$('input[name=xxx]:checked').val();    (xxx不需要有引号包括)
```

   // 获取radio的value值方法二(js)
```
    function GetRadioValue(RadioName){
		var obj;
		obj=document.getElementsByName(RadioName);
		if(obj!=null){
			var i;
			for(i=0;i<obj.length;i++){
				if(obj[i].checked){
					return obj[i].value;
				}
			}
		}
		return null;
	}
```
