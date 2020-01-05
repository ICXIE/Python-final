# Python-final
## title：从国家GDP与工业排放来探究空气污染与两者得相关性
### intro：网站一共分为8个界面，其中包含图表交互和下拉框选择交互及控件跳转，接下来我们将从界面样式，HTML档，Python档及Web App动作这四个方面展开描述。
#### [项目代码GitHub_URL](https://github.com/ICXIE/Python-final)
#### [PythonAnywhere个人部署URL](http://icxie.pythonanywhere.com/)

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
* 发达国家工业污染物排放.html
##### 这7个界面所运用的功能皆运用`<input value='下一页：xxx' type='SUBMIT'>`引入HTML控件实现“下一页”按钮，通过`<form method="POST" action="/x">`实现页面跳转至下一页的对应界面；分别通过使用Python的Plotly中的Pie函数绘制饼图和导入pyecharts类库引用折线图实现图表交互，通过time line map函数实现地图交互。
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
---
#### Python档（app.py）
* 数据传输：
在调用函数的基础上，读取csv数据文件，通过POST方法实现跳转至前端页面，即html返回的具体值。部分代码如下：
```
df1 = pd.read_csv("Industrial_emission_index.csv", encoding='utf-8')
```

```
@app.route('/')
def bing_death():
    return render_template('2017年死亡因素下的死亡人数比例对比.html',
                           the_title='2017年死亡因素下的死亡人数比例对比')
```

* 含有合适的推导式：
```
kqwr = Scatter(
    x=[int(x) for x in dfc.columns],
    y=dfc.loc["空气污染", :].values
)
```
* 含有数据交互（举其中一个例子）：
```
return render_template('hurun.html',
                           the_plot_all=plot_all,
                           the_res=data_str,
                           the_select_region=regions_available,
)
```
* 含有合适的数据结构嵌套：
```
layout = dict(xaxis=dict(rangeselector=dict(buttons=list([
    dict(count=3,
         label="3年",
         step="year",
         stepmode="backward"),
    dict(count=5,
         label="5年",
         step="year",
         stepmode="backward"),
    dict(count=10,
         label="10年",
         step="year",
         stepmode="backward"),
    dict(count=20,
         label="20年",
         step="year",
         stepmode="backward"),
    dict(step="all")
])),
    rangeslider=dict(bgcolor="#70EC57"),
    title='年份'
),
    yaxis=dict(title='世界死亡人数'),
    title="各类空气污染死亡人数对比"
)
```
* 含有合适的列表循环：
```
def timeline_map() -> Timeline:
    tl = Timeline()
    for i in range(2007, 2017):
        map0 = (
            Map()
                .add(
                "", list(zip(list(df2.Country), list(df2["{}年".format(i)]))), "world", is_map_symbol_show=False
            )
                .set_series_opts(label_opts=opts.LabelOpts(is_show=False))
                .set_global_opts(
                title_opts=opts.TitleOpts(title="2007年-2017年各地区GDP对比".format(i), subtitle="",
                                          subtitle_textstyle_opts=opts.TextStyleOpts(color="black", font_size=16,
                                                                                     font_style="italic")),
                visualmap_opts=opts.VisualMapOpts(max_=21972347748529, min_=20439158.42),

            )
        )
        tl.add(map0, "{}年".format(i))
    return tl
```
* 导入pyecharts类库导入图表实现交互：
`from pyecharts import options as opts`
* 定义一个timeline_map函数，符合 PEP8标准：
`def timeline_map()`
---
#### Web App动作
* 下拉框：在hurun.html文件中，我们设置了发达国家的工业碳氧化物排放下拉框选择，通过点击“Do it”提交，网页即呈现对应国家的排放数据；
* 下一页按钮：在html文件中，我们的故事是通过几个方面阐述的，一个表现为一个界面，通过点击下一页按钮即可跳转至下一个分析，举一个例子`<p><input value='总结' type='SUBMIT'></p>`。
