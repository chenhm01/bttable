bootstrap-table填坑之旅<一>认识bootstrap-table
应公司需求，改版公司ERP的数据显示样式。由于前期开发的样式是bootstrap，所以选bootstrap-table理所当然(也是因为看了bootstrap-table官网的example功能强大，样式清爽)。

然后... ... 开启bootstrap-table填坑之旅。

开始就扒本园的资源，确实有不少bootstrap-table的文章。确实写的不错很详细，请恕本菜实在菜了点，看了半天demo的页面都没弄出来(勿吐槽~~)。终于11点了.. .. 于是决定跟着官网的小白教程一点点的玩。

ready... .. 

1. 怎么把table挂出来

HTML代码：(只用看一个tr就够了，写三行只为看demo效果)

复制代码
    <table data-toggle="table" id="goods">
        <thead>
            <tr>
                <th data-field="Code">序号</th>
                <th data-field="TuanGouName">商品名称</th>
                <th data-field="StartDate">开始时间</th>
                <th data-field="EndTime">结束时间</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>1</td>
                <td>九洲奇味饼干</td>
                <td>2016/10/9 10:15:00</td>
                <td>2016/12/25 11:30:00<td>
            </tr>
            <tr>
                <td>2</td>
                <td>好多鱼</td>
                <td>2016/10/9 10:15:00</td>
                <td>2016/12/25 11:30:00<td>
            </tr>
            <tr>
                <td>3</td>
                <td>旺旺雪饼</td>
                <td>2016/10/9 10:15:00</td>
                <td>2016/12/25 11:30:00<td>
            </tr>
         </tbody>
    </table>
复制代码
终于把table挂出来了，这里其实就和原来的table一样写就行了。

2. 加载json数据

HTML代码：

复制代码
    <table id="goods">
        <thead>
            <tr>
                <th data-field="Code">序号</th>
                <th data-field="TuanGouName">商品名称</th>
                <th data-field="StartDate">开始时间</th>
                <th data-field="EndTime">结束时间</th>
            </tr>
        </thead>
    </table>
复制代码
js代码：

复制代码
        /*数据json*/
        var json =  [{"Code":"1","TuanGouName":"好多鱼","StartDate":"2016/10/9 10:15:00","EndTime":"2016/12/25 11:30:00"},
                        {"Code":"2","TuanGouName":"旺旺雪饼","StartDate":"2016/10/9 10:15:00","EndTime":"2016/12/25 11:30:00"},
                        {"Code":"3","TuanGouName":"旺旺仙贝","StartDate":"2016/10/9 10:15:00","EndTime":"2016/12/25 11:30:00"},
                        {"Code":"4","TuanGouName":"雪花清爽","StartDate":"2016/10/9 10:15:00","EndTime":"2016/12/25 11:30:00"},
                        {"Code":"5","TuanGouName":"勇闯天涯","StartDate":"2016/10/9 10:15:00","EndTime":"2016/12/25 11:30:00"},
                        {"Code":"6","TuanGouName":"九洲奇味饼干","StartDate":"2016/10/9 10:15:00","EndTime":"2016/12/25 11:30:00"}];
        /*初始化table数据*/
        $(function(){
            $("#goods").bootstrapTable({
                data:json
            });
        });
复制代码
成功获取json数据并加载成功。

这里注意：用json加载数据 table 标签不能写 data-toggle="table" 属性，至于原因......总之这里不能写，写了就会出这样的bug。



3. 装饰table

HTML代码

复制代码
<table data-toggle="table" 
       data-row-style="rowStyle">
    <thead>
    <tr>
        <th class="col-xs-4" data-field="name">Name</th>
        <th class="col-xs-1" data-field="stargazers_count">Stars</th>
        <th class="col-xs-1" data-field="forks_count">Forks</th>
        <th class="col-xs-6" data-field="description">Description</th>
    </tr>
    </thead>
    <!--此处忽略数据-->
</table>
复制代码
js代码

复制代码
function rowStyle(value, row, index) {
    var classes = ['active', 'success', 'info', 'warning', 'danger'];
    if (index % 2 === 0) {
        return { classes: 'success' };
    }
    return {};
}
复制代码
data-striped属性true表格为隔行变色(斑马纹)，false不使用隔行变色。

data-row-style属性接收js函数(必须有返回值)，可设置row属性。

th每列可添加栅格样式。

在th可设置data-cell-style属性，同样接收js函数(必须有返回值)，设置该列单元格样式。

HTML代码

复制代码
<table data-toggle="table" >
    <thead>
    <tr>
        <th data-field="name" data-halign="right" data-align="center">Name</th>
        <th data-field="stargazers_count" data-halign="center" data-align="left">Stars</th>
        <th data-field="forks_count" data-halign="left" data-align="right">Forks</th>
    </tr>
    </thead>
</table>
复制代码
data-halign设置该列标题对齐，data-align设置该列单元格对齐。

分组列显示——colspan & rowspan

<table id="table"></table>
js

复制代码
        /*列信息*/
        var firstCol = [
                        [{"field":"goodsName","title":"商品名称","colspan":1,"rowspan":2},
                        {"title":"商品信息","colspan":2,"rowspan":1}],
                        [{"field":"goodsInfo.price","title":"价格","colspan":1,"rowspan":1},
                         {"field":"goodsInfo.date","title":"日期","colspan":1,"rowspan":1}]
                         ];
        /*数据*/
        var data = [{"goodsName":"旺旺仙贝","goodsInfo":{"price":"$26","date":"2018-08-10"}},
                    {"goodsName":"乐事薯片","goodsInfo":{"price":"$18","date":"2020-10-25"}},
                    {"goodsName":"勇闯天涯","goodsInfo":{"price":"$20","date":"2017-01-10"}}];
        /*初始化表格*/
        $(function(){
            $("#goods").bootstrapTable({
                columns: firstCol,
                data: data
            });
        });
复制代码
分组列组名不需要申明field值，但分组列子列的field值需要带上列组名(格式：Group.GroupChild)。如果分组列，数据的json也需要做相应的调整。

4. table排序

HTML代码

复制代码
    <table id="goods" data-sort-order="desc">
        <thead>
            <tr>
                <th data-sortable="true" data-field="Code">序号</th>
                <th data-sortable="true" data-field="TuanGouName">商品名称</th>
                <th data-field="StartDate">开始时间</th>
                <th data-field="EndTime">结束时间</th>
            </tr>
        </thead>
    </table>
复制代码
data-sortable属性默认为false，设置为true，按默认排序方式对该列内容排序。

data-sort-order排序方向，asc升序排列、desc降序排列。

data-sort-name="stargazers_count"这俩属性找了半天没找到准确的解释，从字面意思理解应该是默认的排序函数名和排序方式，总之带上总没错。

5. 单元格 格式化

HTML代码

复制代码
    <table id="goods" data-sort-name="stargazers_count"
       data-sort-order="asc">
        <thead>
            <tr>
                <th data-sortable="true" data-formatter="getIndex">下标</th>
                <th data-sortable="true" data-field="Code" data-formatter="setCode">序号</th>
                <th data-sortable="true" data-field="TuanGouName" data-formatter="setName">商品名称</th>
                <th data-sortable="true" data-field="name">名称</th>
                <th data-field="EndTime">结束时间</th>
            </tr>
        </thead>
    </table>
复制代码
js代码

复制代码
        function getIndex(val,row,index){
            return index + 1;
        }
        function setCode(val){
            return "<a href='#'>" + val + "</a>";
        }
        function setName(val){
            return "<span style='color:red;font-weight:900;'>" + val + "</span>";
        }
复制代码
data-formatter属性可以格式化该列单元格，data-formatter接收js函数(必须有返回值)该函数可以获取当前行的下标(注意：获取下标参数必须有row，否则index值为undefined)，函数还可以改变单元格元素显示方式，例如：a button .. ...

6. 显示隐藏列

HTML代码

复制代码
    <table id="goods" data-sort-name="stargazers_count" data-show-columns="true"
       data-sort-order="asc">
        <thead>
            <tr>
                <th data-sortable="true" data-field="Code" >序号</th>
                <th data-sortable="true" data-field="TuanGouName" data-switchable="false">商品名称</th>
                <th data-sortable="true" data-field="name">名称</th>
                <th data-sortable="true" data-field="EndTime" data-visible="false">结束时间</th>
            </tr>
        </thead>
    </table>
复制代码
data-show-columns属性为“true”可设置隐藏显示某列，对应列data-switchable属性设置为“false”该列不可隐藏，默认值为true；data-visible属性设置为“false”该列默认被隐藏，默认值为true。

7. 选择列 checkbox

HTML代码

复制代码
    <table id="goods" data-sort-name="stargazers_count" data-show-columns="true"
           data-sort-order="asc">
        <thead>
            <tr>
                <th data-field="state" data-checkbox="true"></th>
                <th data-sortable="true" data-field="Code" >序号</th>
                <th data-sortable="true" data-field="TuanGouName" data-switchable="false">商品名称</th>
                <th data-sortable="true" data-field="name">名称</th>
                <th data-sortable="true" data-field="EndTime" data-visible="false">结束时间</th>
            </tr>
        </thead>
    </table>
复制代码
<th data-field="state" data-checkbox="true"></th>这一列是checkbox选择列。据测试，data-click-to-select属性的值与选择列关系不大，有木有或者值true false都不影响checkbox列的显示和使用。

设置data-single-select="true"，checkbox就只能选择一行。

复制代码
    <table id="goods" data-sort-name="stargazers_count" data-show-columns="true"
           data-sort-order="asc" data-click-to-select="true" data-single-select="true">
        <thead>
            <tr>
                <th data-field="state" data-checkbox="true">选择</th>
                <th data-sortable="true" data-field="Code" >序号</th>
                <th data-sortable="true" data-field="TuanGouName" data-switchable="false">商品名称</th>
                <th data-sortable="true" data-field="name">名称</th>
                <th data-sortable="true" data-field="EndTime" data-visible="false">结束时间</th>
            </tr>
        </thead>
    </table>
复制代码
通过js指定行被选中，指定行不可操作。

复制代码
function stateFormatter(value, row, index) {
    if (index === 2) {
        return {
            disabled: true
        };
    }
    if (index === 0) {
        return {
            disabled: true,
            checked: true
        }
    }
    return value;
}
复制代码
给checkbox设置data-formatter属性，通过disabledchecked控制checkbox是否可用和是否被选中。

获取选中行信息

html

    <div class="toolbar">
        <button id="get" class="btn btn-default">获取商品名称</button>
    </div>
    <table id="table" data-show-columns="true" data-height="700" data-toolbar=".toolbar"></table>
js

复制代码
        /*获取选中行对象*/
        function getContent(){
            var index = $("#table").find("tr.danger").data("index");
            return $("#table").bootstrapTable('getData')[index];
        }
        /*初始化table数据*/
        $(function(){
            $("#table").bootstrapTable('destroy').bootstrapTable({
                columns:columns,
                data:json
            });
            $("#table").on("click-row.bs.table",function(e,row,ele){
                $(".danger").removeClass("danger");
                $(ele).addClass("danger");
            });
            $("#get").click(function(){
                alert("商品名称：" + getContent().TuanGouName);
            })
        });
复制代码
给table绑定click-row.bs.table函数(行点击事件)，callback(回调)函数列表：e(Event：事件对象)，row(Rows：table行)，ele(Element：选中行对象)。给选中行添加颜色样式，移除上一个被选行样式。

getContent()函数分析：

var index 获取被选中行下表，find搜索被选中行(即带样式的行)，data被选中行在数据集中的下标。

return 返回table中被选中行对象。

点击查询按钮click事件：

既然getContent()已获取被选中行对象，需要获取哪个单元格，就调哪个单元格的field值。

8. card-view 卡片视图

HTML代码

复制代码
<table data-toggle="table"
       data-card-view="true">
    <thead>
    <tr>
        <th data-field="name">Name</th>
        <th data-field="stargazers_count">Stars</th>
        <th data-field="forks_count">Forks</th>
        <th data-field="description">Description</th>
    </tr>
    </thead>
</table>
复制代码
data-card-view改变table视图方式，true:卡片视图，false:表格视图。

9. toolbar工具栏(常用 搜索 刷新 切换试图 筛选列)

HTML代码

复制代码
    <table id="goods"
        data-search="true"
                data-show-refresh="true"
                data-show-toggle="true"
                data-show-columns="true">
        <thead>
            <tr>
                <th data-sortable="true" data-field="Code" >序号</th>
                <th data-sortable="true" data-field="TuanGouName" data-switchable="false">商品名称</th>
                <th data-sortable="true" data-field="name">名称</th>
                <th data-sortable="true" data-field="EndTime" data-visible="false">结束时间</th>
            </tr>
        </thead>
    </table>
复制代码
data-search：搜索(自动搜索，输入后自动搜索)

data-show-refresh：刷新

data-show-toggle：切换试图(卡片试图 and 表格试图)

data-show-columns：筛选列

自定义添加工具栏按钮

复制代码
<div id="toolbar" class="btn-group">
    <button type="button" class="btn btn-default">
        <i class="glyphicon glyphicon-plus"></i>
    </button>
    <button type="button" class="btn btn-default">
        <i class="glyphicon glyphicon-heart"></i>
    </button>
    <button type="button" class="btn btn-default">
        <i class="glyphicon glyphicon-trash"></i>
    </button>
</div>
<table data-toggle="table"
       data-toolbar="#toolbar">
    <thead>
    <tr>
        <th data-field="name">Name</th>
        <th data-field="stargazers_count">Stars</th>
        <th data-field="forks_count">Forks</th>
        <th data-field="description">Description</th>
    </tr>
    </thead>
</table>
复制代码
data-toolbar添加自定义工具栏(value建议为ID值)。

10.分页pagination

HTML代码

    <table id="goods" data-query-params="queryParams" data-pagination="true"></table>
js代码

复制代码
        function queryParams() {
            return {
                type: 'owner',            
                sort: 'updated',     
                direction: 'desc',            //排序方向
                per_page: 10,                //一次加载数据条数
                page:1                        //加载数据第几次
            };
        }
复制代码
data-page-list定义每页显示条数，接受数组。例：data-page-list="[2,4,6,10,20]"

data-pagination值为true，表格使用分页，data-query-params分页配置参数，接受js函数(必须有返回值)。

direction排序方向：asc升序，desc降序

per_page一次性加载数据条数：int整数

page请求数据次数

例：如共有190条数据，page值为1，per_page值为100。table加载第1~100条数据，

page值为2，per_page值为100。table加载第201~300条数据。

注意：data-query-params仅对请求数据地址有对应参数的返回值才生效，对json拉取到本页解析的数据和本页直接生成的数据皆无效。详细DEMO地址，不懂的自己多调试几遍，这里我调了半个小时。

关于服务器端分页请参考这个demo

更多详细还是看官方文档：地址

标签: bootstrap
好文要顶 关注我 收藏该文    
MirageFireFox
关注 - 5
粉丝 - 4
+加关注
0 0
« 上一篇：CSS3打造3D效果——perspective transform的深度剖析
» 下一篇：bootstrap-table填坑之旅<二>事件
posted on 2016-10-23 12:34 MirageFireFox 阅读(11442) 评论(10) 编辑 收藏

FeedBack:
#1楼
2017-01-11 16:35 | think_D  
第二点 在加载json数据后添加了toggle是正常的，出错原因可能是其他吧，或者版本问题。我这里是OK的
支持(0)反对(0)
  回复引用
#2楼[楼主]
2017-01-12 20:40 | MirageFireFox  
@ think_D
你用的哪个版本，我又试了下 还是有问题。
支持(0)反对(0)
  回复引用
#3楼
2017-01-13 11:36 | think_D  
@ MirageFireFox
　　
<table id="table-custom-sort" 
                                        data-toggle="table"
                                        data-height="600" 
                                        data-sort-name="id" 
                                        data-sort-order="asc"
                                        data-search=true 
                                        data-striped=true 
                                        data-pagination=true
                                        data-only-info-pagination=true 
                                        data-show-columns=true>
                                        <thead>
                                            <tr>
                                                <th data-field="name" 
                                                    data-align="center"
                                                    data-sortable="true">name</th>
                                                <th data-field="bulid" 
                                                    data-align="center"
                                                    data-sortable="true">bulid</th>
                                                <th data-field="sum_steps" 
                                                    data-align="center"
                                                    data-sortable="true">操作步数</th>
                                                <th data-field="sum_worktime" 
                                                    data-align="center"
                                                    data-sortable="true">操作时长</th>
                                                <th data-field="date" 
                                                    data-align="center"
                                                    data-sortable="true">日期</th>
                                                <th data-field="href" 
                                                    data-align="left" 
                                                    data-sortable="true">查看</th>
                                            </tr>
                                        </thead>
                                    </table>

------------------------------------------------------------------------------------------------------
$('#table-custom-sort').bootstrapTable({
data : data,
});
-----------------------------------------------------------------------------------------------------
上面是我自己的代码 HTM了和JS调起
版本是2017年1月13日最近在官网下载的最新版 具体多少我也忘记了。
支持(0)反对(0)
  回复引用
#4楼
2017-01-18 17:54 | think_D  
@ MirageFireFox
你那面出现的的具体问题是什么？
我这里也除了一点小问题，也是去掉那个就OK，但不是完美解决方案。
支持(0)反对(0)
  回复引用
#5楼
2017-01-23 15:58 | 僵小徐  
32个赞，必须收了，正在找这个
支持(0)反对(0)
  回复引用
#6楼
2017-01-23 17:41 | 僵小徐  
大神，在这段代码里，看API，没有这个value这个参数啊，这里多了个这个是什么意思啊？而且，我自己写的代码，如果加上这个参数，就出不来效果了。求解
function rowStyle(value, row, index) {
var classes = ['active', 'success', 'info', 'warning', 'danger'];
if (index % 2 === 0) {
return { classes: 'success' };
}
return {};
}
支持(0)反对(0)
  回复引用
#7楼
2017-01-23 17:41 | 僵小徐  
@ think_D
我也有问题，我用的最新的
支持(0)反对(0)
  回复引用
#8楼[楼主]
2017-02-04 10:29 | MirageFireFox  
@ 僵小徐
value API上有啊，是值，不记得是单元格的值还是数据源的值，我找的时候API上有value
支持(0)反对(0)
  回复引用
#9楼
2017-02-06 08:50 | 僵小徐  
可能是版本不同吧，现在没有这个值了，再请教个问题，怎么修改单元格样式
支持(0)反对(0)
  回复引用
#10楼[楼主]
2017-02-06 13:15 | MirageFireFox  
@ 僵小徐
你指的什么样式？data-cell-style这个可以设置table的css样式