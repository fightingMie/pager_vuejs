# pager_vuejs
#### vuejs分页组件说明

##### 参数说明：

###### props ['totalPager']
-	子组件在父组件中获取分页总页数。

###### data () 中数据：
-	limitPager：分页限制超过该限制值，显示省略号
-	preClipped：超出限制值以后，分页之前的省略号
-	nextClipped：超出限制以后，分页后面的省略号
-	currentPager：当前页默认1

##### style内容说明
-	分页样式和class名可以根据需要自定义设置
