# Python-final
## title：从国家GDP与工业排放来探究空气污染与两者得相关性
### intro：网站一共分为8个界面，其中包含图表交互和下拉框选择交互及控件跳转，接下来我们将从界面样式，HTML档，Python档及Web App动作这四个方面展开描述。
---
#### 界面样式
* 背景：渐变色`background-image: linear-gradient(120deg, #a6c0fe 0%, #f68084 100%);`
* 布局：标题居中`text-align: center;`
---
#### HTML档
* 2017年死亡因素下的死亡人数比例对比.html
* 空气污染死亡人数对比折线图.html
* 2007年-2017年各地区GDP对比.html
* 2007年-2017年空气污染导致的死亡率影响最大的地区.html
* 2010年-2016年各地区空气污染浓度.html
* 2016年pm2.5指数最高的国家与pm2.5指数最小的国家对比.html
##### 这6个界面所运用的功能皆运用`<input value='下一页：xxx' type='SUBMIT'>`引入HTML控件实现“下一页”按钮，通过`<form method="POST" action="/x">`实现页面跳转至下一页的对应界面；分别通过使用Python的Plotly中的Pie函数绘制饼图和导入pyecharts类库引用折线图实现图表交互，通过time line map函数实现地图交互。
* hurun.html
1. 数据循环/嵌套：`{% for item in the_select_region %}`遍历“the_region_selected”内的数据，在网页内获取其所有的内部元素，实现表格交互；
	               `{% for item in the_select_region %}{% endfor %}`运用for循环和列表结构；
                 `regions_available = list(df1.country.dropna().unique())`属于数据嵌套。
2. 与python文档的交互体现：通过`select`和`option`创建多选下拉框的功能实现筛选地区，以实现显示对应地区的详细数据的交互。部分代码块如下：
```
<select name="the_region_selected">
  {% for item in the_select_region %}
  <option value="{{ item }}">{{ item }}</option>
  {% endfor %}
</select>
```
3. jinja2模板运用：
```
{{ the_plot_all|safe }}
{{ the_res|safe }}
```
* summary.html
1. 样式上插入轮播图。
2. `<link rel="stylesheet" href="https://cdn.staticfile.org/twitter-bootstrap/4.3.1/css/bootstrap.min.css">`为 链接css。
