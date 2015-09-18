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
    '<span><img src="vendor/images/flags/' + state.element.value.toLowerCase() + '.png" class="img-flag" /> ' + state.text + '</span>'
  );
  return $state;
};
 
$(".js-example-templating").select2({
  templateResult: formatState,
  templateSelection:formatState
});
```

4、支持加载搜索远程数据。
** html代码：***
```html
<select class="js-data-example-ajax" name="catId"><option value="3620194" selected="selected">select2/select2</option></select>
```
**Javascript代码部分：**
```javascript
$(".js-data-example-ajax").select2({
		      ajax: {
		        url: "/opms/category/list.json",
		        dataType: 'json',
		        delay: 250,
		        data: function (params) {
		          return {
		            name: params.term, // search term
		            limit: 999
		            // page: params.page
		          };
		        },
		        processResults: function (data, page) {
		          // parse the results into the format expected by Select2.
		          // since we are using custom formatting functions we do not need to
		          // alter the remote JSON data
		          console.log(data);
		          console.log(page);
		          var res = [];
		          if(data.code==0 && data.data.list.length>0){
		          	res = data.data.list;
		          }
		          return {
		            results: res,
		            pagination: {
			            more: (data.data.pageNum * 30) < data.data.totalCount
			          }
		          };
		        },
		        cache: true
		      },
		      escapeMarkup: function (markup) { return markup; }, // let our custom formatter work
		      minimumInputLength: 1,
		      templateResult: formatRepo, // omitted for brevity, see the source of this page
		      templateSelection: formatRepoSelection // omitted for brevity, see the source of this page
		    });
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
			function formatRepoSelection (repo) {
				return repo.aliasName || repo.text;
			}
```




