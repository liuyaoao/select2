# select2
##jquery插件select2的学习解析

1、最简单的使用；
  $(document).ready(function() {
  $(".js-example-basic-single").select2();
});
<select class="js-example-basic-single">
  <option value="1">Alabama</option>
    ...
  <option value="2">Wyoming</option>
</select>

2、Multiple select boxes ：可多选的下拉列表。
  用法：只要给select标签加一个multiple属性，如下代码所示；
  ```html
  <select class="js-example-basic-multiple" multiple="multiple">
  <option value="AL">Alabama</option>
    ...
  <option value="WY">Wyoming</option>
</select>
```

3、Templating ；给下拉列表的待选结果列表和已选中区域使用自定义的html模板;
  用法： 只要给select2()方法的两个属性设置就行了，这两个属性的值分别是两个函数，下拉选项模板对应的是templateResult属性；已选中的区域对应的模板是templateSelection属性；可以设置这两个属性为同一个函数，其实最好是这样，不然选中展示的会和下拉列表里展示的不一样。
  
**代码模板：**
```javascript
  function formatState (state) {
  if (!state.id) { return state.text; }
  var $state = $(
    '<span><img src="vendor/images/flags/' 
    + state.element.value.toLowerCase() 
    + '.png" class="img-flag" /> ' 
    + state.text + '</span>'
  );
  return $state;
};
 
$(".js-example-templating").select2({
  templateResult: formatState,
  templateSelection:formatState
});
```

4、支持加载搜索远程数据。这个其实不好支持初始化设值，更多的是用来搜索取值用。

**html代码：**
```html
<select class="js-data-example-ajax" name="catId">
	<option value="3620194" selected="selected">select2/select2</option>
</select>
```
**Javascript代码部分：**
```javascript
$(".js-data-example-ajax").select2({
		      ajax: {
		        url: "/opms/category/list.json",
		        dataType: 'json',
		        delay: 250,
		        data: function (params) { ///获取查询对象参数。
		          return {
		            name: params.term, // 这个是直接获取输入文本框里的查询参数。
		            limit: 999    // 这是给查询接口加一些其他字段的参数。
		            // page: params.page
		          };
		        },
		        ///处理查询结果，其中data就是后端返回的结果，有可能要自己重新解析一下。
		        ///page参数就是原来的查询参数对象。
		        processResults: function (data, page) {
		          console.log(data);
		          console.log(page);
		          var res = [];
		          if(data.code==0 && data.data.list.length>0){
		          	res = data.data.list;
		          }
		          return {
		            results: res, ///返回给select2组件的结果数据，是一个对象数组。
		            pagination: {  ///控制是否分页显示结果。这里后端会返回页数和总个数。
			            more: (data.data.pageNum * 30) < data.data.totalCount
			          }
		          };
		        },
		        cache: true
		      },
		      escapeMarkup: function (markup) { return markup; }, // let our custom formatter work
		      minimumInputLength: 1,
		      templateResult: formatRepo, // 下拉列表里带选择结果的模板函数。
		      templateSelection: formatRepoSelection // 已选择出的结果显示的模板函数。
		    });
		    	///这个是控制下拉列表显示的模板。
		    	////这里的参数就是后端返回的每一个数据对象。里面包含很多参数字段。
			function formatRepo (repo) {
			    if (repo.loading) return repo.text;

			    var markup = '<div class="clearfix">' +
			    '<div clas="col-sm-10">' +
			    '<div class="clearfix">' +
			    '<div class="col-sm-6">' + repo.aliasName + '</div>' +
			    '<div class="col-sm-3"><i class="fa fa-code-fork"></i> ' + repo.name + '</div>' +
			    '<div class="col-sm-2"><i class="fa fa-star"></i> ' + repo.id + '</div>' +
			    '</div>';

			    if (repo.description) {
			      markup += '<div>' + repo.description + '</div>';
			    }

			    markup += '</div></div>';

			    return markup;
			}
			///这个是控制在最上面已选中的结果显示的模板。
			function formatRepoSelection (repo) {
				return repo.aliasName || repo.text;
			}
```




