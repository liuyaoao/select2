# select2
jquery插件select2的学习解析

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
  <select class="js-example-basic-multiple" multiple="multiple">
  <option value="AL">Alabama</option>
    ...
  <option value="WY">Wyoming</option>
</select>

3、Templating ；给下拉列表的待选结果列表和已选中区域使用自定义的html模板;
  用法： 只要给select2()方法的两个属性设置就行了，这两个属性的值分别是两个函数，下拉选项模板对应的是templateResult属性；已选中的区域对应的模板是templateSelection属性；可以设置这两个属性为同一个函数，其实最好是这样，不然选中展示的会和下拉列表里展示的不一样。
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





