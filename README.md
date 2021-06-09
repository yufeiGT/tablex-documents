### tablex

> tablex 是一个基于 [Vue](https://cn.vuejs.org/) 、[ELement UI](https://element.eleme.cn/#/zh-CN) 、 [Axios](http://www.axios-js.com/) 实现的一个 table 组件，可根据配置快速实现增删改查业务

----

#### 安装

```javascript
npm i @~crazy/tablex -S
```

----

#### 使用

* main.js

```javascript
// 引入
import tablex from '@~crazy/tablex'
// 安装
Vue.use(tablex, {
    // 全局配置
});
// 全局访问 tablex 对象
Vue.$tablex;
```

----

#### tablex 全局属性

|属性名|说明|
|:-|:-|
|version|版本号|
|api|接口对象，根据 request.map 配置生成的接口函数|
|responseFormat|全局请求响应数据格式化回调|
|options|配置|
|type|类型对象|

----

#### 全局配置

|配置名|类型|默认值|说明|
|:-|:-|:-|:-|
|i18n|Object|-|Vue I18n 实例，实现组件国际化|
|locales|Object|-|国际化语言列表|
|store|Obejct|-|Vue Store 实例|
|request|Object|-|请求配置|
|request.map|Object|-|接口地图|
|request.axios|Object|-|Axios 配置|
|request.axios.authorizationKey|String|Authorization|在请求头携带的 Token 认证名称|
|request.axios.defaults|Object|-|同 [Axios](http://www.axios-js.com/) 配置|
|request.axios.defaults.timeout|Number|1000 * 5|请求超时毫秒数|
|request.axios.defaults.withCredentials|Boolean|true|跨域请求是否需要使用凭证|
|request.axios.defaults.headers|Object|{<br>&nbsp;&nbsp;&nbsp;&nbsp;'Content-Type': 'application/json;charset=UTF-8'<br>}|请求头配置|
|request.axios.interceptors|Object|-|自定义拦截处理回调|
|request.axios.interceptors.request|Array[Function]|[]|请求处理回调列表|
|request.axios.interceptors.response|Array[Function]|[]|响应处理回调列表|
|table|Object|-|Table 组件配置|
|table.requestFormat|Function|-|全局请求参数格式化回调|
|table.responseFormat|Function|-|全局响应数据格式化回调|
|table.deleteRequestFormat|Function|-|全局删除请求参数格式化回调|
|table.delete|Obejct|-|删除行动配置|
|table.delete.key|String|id|发送请求时参数的名称|
|table.delete.paramKey|String|id|获取参数时数据列的名称|
|table.paging|Obejct|-|分页配置|
|table.paging.paramsFormat|Function|-|分页参数格式化回调|

```javascript
Vue.use(tablex, {
    // Vue I18n
    i18n: null,
    // 国际化语言列表
    locales: {},
    // Vue Store
    store: null,
    // 请求配置
    request: {
        // 接口地图
        map: {},
        // axios 配置
        axios: {
            authorizationKey: 'Authorization',
            // 全局配置
            defaults: {
                // 请求超时毫秒数
                timeout: 1000 * 5,
                // 跨域请求是否需要使用凭证
                withCredentials: true,
                // 请求头配置
                headers: {
                    'Content-Type': 'application/json;charset=UTF-8',
                },
            },
            // 自定义拦截处理回调
            interceptors: {
                request: [],
                response: [],
            },
        },
    },
    table: {
        // 全局请求参数格式化回调
        requestFormat: null,
        // 全局响应数据格式化回调
        responseFormat: null,
        // 全局删除请求参数格式化回调
        deleteRequestFormat: null,
        // 删除
        delete: {
            // 参数键
            key: 'id',
            // 获取参数值的键
            paramKey: 'id',
        },
        // 分页
        paging: {
            // 分页参数格式化回调
            paramsFormat: null,
        },
    },
})
```

----

#### request.map 接口地图

> 格式为 JSON，接口集合可多层嵌套

* 格式

```javascript
{
    接口名称：'请求方式，GET / POST',
    // 如 POST ： http://XXXX/login
    login: 'post',
    
    自定义接口函数名称： ['请求方式，GET / POST', '接口名称'],
    // 如 POST ：http://XXXX/login
    // 自定义接口函数名为 Login
    Login: ['post', 'login'],
    
    接口集合名(以开头的名称命名)：{},
    // 如有以下以 system 开头的接口:
    // POST：http://XXXX/system/setUser
    // GET：http://XXXX/system/getUserData
    system: {
        setUser: 'post',
        getUserData: 'get',
    },
    
    接口集合名(不以开头的名称命名)：{},
    // 如有以下以 system 开头的接口:
    // POST：http://XXXX/system/setUser
    // GET：http://XXXX/system/getUserData
    SystemData: {
        // 设置默认接口前缀
        default: 'system',
        setUser: 'post',
        getUserData: 'get',
    },
}
```

* 示例

```javascript
import tablex from '@~crazy/tablex';
// 配置
Vue.use(tablex, {
    request: {
        map: {
            // 设置用户数据
            setUserData: ['post', 'set_user_data'],
        },
    },
});

// 通过 api 属性访问
// 获取请求实例
const req = this.$tablex.api.setUserData({
    // 传递参数
    nickname: 'admin',
    address: 'xxxxxx',
});
// 发送请求
req.send({
    // 此处可覆盖参数
}).then(res => {
    // 请求成功
});

// 取消请求
req.cancel();
```

----

#### 数据类型

> 为了实现各种数据类型在界面上的展示(Visible)、编辑（Edit），在此定义了几种数据类型（文档中为了不与原生属性混淆，使用 tablex 作为前缀）：

##### Type

基类，所有类型均继承此类。目前有以下类：

|类型|继承于|说明|
|:-|:-|:-|
|tablex.String|tablex.Type|字符串|
|tablex.JSON|tablex.Type|json 对象，尚未实现|
|tablex.Double|tablex.Type|浮点数|
|tablex.Password|tablex.String|密码|
|tablex.Date|tablex.Type|日期，YYYY-MM-DD HH:mm:ss|
|tablex.Time|tablex.Date|时间点，HH:mm:ss|
|tablex.DateRange|tablex.Type|时间区间，YYYY-MM-DD HH:mm:ss - YYYY-MM-DD HH:mm:ss|
|tablex.Select|tablex.Type|下拉选择器|
|tablex.Radio|tablex.Select|单选|
|tablex.Checkbox|tablex.Radio|多选|

###### Type 类实例有以下函数：

> 不同类型函数传参有所不同

* constructor 构造函数 & set

<table>
    <theader>
        <tr>
            <th>参数</th>
            <th>类型</th>
            <th>默认值</th>
            <th>适用类型</th>
            <th>参数顺序</th>
            <th>说明</th>
        </tr>
    </theader>
    <tbody>
        <tr>
            <td>value</td>
            <td>Any</td>
            <td>-</td>
            <td>所有</td>
            <td>-</td>
            <td>原始数据值，不同类型会根据 value 作出不同的显示方式</td>
        </tr>
        <tr>
            <td rowspan="3">formatString</td>
            <td rowspan="3">String</td>
            <td>YYYY-MM-DD HH:mm:ss</td>
            <td>tablex.Date</td>
            <td rowspan="2">(value, formatString)</td>
            <td rowspan="3">格式化显示方式，不包含 Edit 状态</td>
        </tr>
        <tr>
            <td>HH:mm:ss</td>
            <td>tablex.Time</td>
        </tr>
        <tr>
            <td>YYYY-MM-DD HH:mm:ss</td>
            <td>DateRange</td>
            <td>(start, end, formatString)</td>
        </tr>
        <tr>
            <td>start</td>
            <td>String/Number/Date</td>
            <td>-</td>
            <td>tablex.DateRange</td>
            <td>(start, end, formatString)</td>
            <td>开始时间</td>
        </tr>
        <tr>
            <td>end</td>
            <td>String/Number/Date</td>
            <td>-</td>
            <td>tablex.DateRange</td>
            <td>(start, end, formatString)</td>
            <td>结束时间</td>
        </tr>
        <tr>
            <td rowspan="3">options</td>
            <td rowspan="3">Array[Object({<br>&nbsp;&nbsp;&nbsp;&nbsp;lable: String,<br>&nbsp;&nbsp;&nbsp;&nbsp;value: String,<br>})]</td>
            <td rowspan="3">[]</td>
            <td>tablex.Select</td>
            <td rowspan="3">(value, options)</td>
            <td rowspan="3">数据列表</td>
        </tr>
        <tr>
            <td>tablex.Radio</td>
        </tr>
        <tr>
            <td>tablex.Checkbox</td>
        </tr>
    </tbody>
</table>

* getEditor

> 获取类型编辑器，编辑器可修改数据值返回到数据源中。多用于表单提交新增、修改功能

<table>
    <theader>
        <tr>
            <th>参数</th>
            <th>类型</th>
            <th>默认值</th>
            <th>适用类型</th>
            <th>参数顺序</th>
            <th>说明</th>
        </tr>
    </theader>
    <tbody>
        <tr>
            <td>vue</td>
            <td>Vue</td>
            <td>-</td>
            <td>所有</td>
            <td>-</td>
            <td>Vue 实例对象</td>
        </tr>
        <tr>
            <td rowspan="9">props</td>
            <td rowspan="9">Object</td>
            <td rowspan="9">-</td>
            <td>tablex.String</td>
            <td rowspan="9">(vue, props)</td>
            <td rowspan="2">同 el-input 组件参数</td>
        </tr>
        <tr>
            <td>tablex.Password</td>
        </tr>
        <tr>
            <td>tablex.Double</td>
            <td>同 el-input-number 组件参数</td>
        </tr>
        <tr>
            <td>tablex.Date</td>
            <td rowspan="2">同 el-date-picker 组件参数</td>
        </tr>
        <tr>
            <td>tablex.DateRange</td>
        </tr>
        <tr>
            <td>tablex.Time</td>
            <td>同 el-time-picker 组件参数</td>
        </tr>
        <tr>
            <td>tablex.Select</td>
            <td>同 el-select 组件参数</td>
        </tr>
        <tr>
            <td>tablex.Radio</td>
            <td>同 el-radio 组件参数</td>
        </tr>
        <tr>
            <td>tablex.Checkbox</td>
            <td>同 el-checkbox 组件参数</td>
        </tr>
    </tbody>
</table>

* toVisible

> 将原始数据转换为显示状态

<table>
    <theader>
        <tr>
            <th>参数</th>
            <th>类型</th>
            <th>默认值</th>
            <th>适用类型</th>
            <th>参数顺序</th>
            <th>说明</th>
        </tr>
    </theader>
    <tbody>
        <tr>
            <td>vue</td>
            <td>Vue</td>
            <td>-</td>
            <td>所有</td>
            <td>-</td>
            <td>Vue 实例对象</td>
        </tr>
        <tr>
            <td rowspan="2">placeholder</td>
            <td rowspan="2">String</td>
            <td rowspan="2">-</td>
            <td>tablex.String</td>
            <td rowspan="2">(vue, placeholder)</td>
            <td rowspan="2">占位文本</td>
        </tr>
        <tr>
            <td>tablex.Password</td>
        </tr>
        <tr>
            <td>precision</td>
            <td>Number</td>
            <td>0</td>
            <td>tablex.Double</td>
            <td>-</td>
            <td>精度</td>
        </tr>
        <tr>
            <td rowspan="3">formatString</td>
            <td rowspan="3">String</td>
            <td rowspan="3">-</td>
            <td>tablex.Date</td>
            <td rowspan="3">(vue, formatString)</td>
            <td rowspan="3">格式化显示方式，为空时使用实例的 formatString 值</td>
        </tr>
        <tr>
            <td>tablex.Time</td>
        </tr>
        <tr>
            <td>tablex.DateRange</td>
        </tr>
    </tbody>
</table>

* toJson

> 将原始数据转换为 JSON 对象

|参数|类型|默认值|说明|
|:-|:-|:-|:-|
|key|String|-|原始值的键|
|keyAlias|String/Array[String]|-|需要将原始值转化的别名键|
|value|Any|-|原始值|

----


####  tablex 组件

|配置名|类型|说明|
|:-|:-|:-|
|options|Object|tablex 组件配置，tablex 依靠此配置工作|
|tablex|tablex 实例|父级 tablex 实例，在多个 tablex 嵌套使用插槽时传入|

* 示例

```html
<template>
    <!-- 引用 -->
    <el-tablex :options="options"></el-tablex>
</template>
<script>
export default {
    computed: {
        options: {
            // tablex 组件配置
        },
    },
};
</script>
```

* 多个 tablex 嵌套

```html
<template>
    <!-- 引用 -->
    <el-tablex :options="options">
        <!-- 嵌套的 tablex 使用插槽，options 与 tablex 属性需要使用作用域变量传入 -->
        <el-tablex slot="table-details" slot-scope="scope" :options="scope.options" :tablex="scope.tablex"></el-tablex>
    </el-tablex>
</template>
<script>
export default {
    computed: {
        options: {
            // 省略部分代码...
            actions: {
                // 查看详情列表
                details: {
                    table: {
                        // 此处为嵌套 tablex
                    },
                },
            },
        },
    },
};
</script>
```

##### tablex 组件配置 tablex.Options (options 属性)

|配置名|类型|默认值|说明|
|:-|:-|:-|:-|
|multiple|Boolean|false|开启列表多选功能|
|model|Object(tablex.Model)|-|数据模型|
|data|Array|null|列表数据|
|actions|Object(tablex.Action)|-|行为列表|
|actionParams|Object|-|全局行为请求时附带参数|
|requestFormat|Function|null|全局请求参数格式化回调|
|responseFormat|Function|null|全局响应数据格式化回调|
|deleteRequestFormat|Function|null|全局删除请求参数格式化回调|
|condition|Object|-|查询条件|
|condition.showButton|Boolean|true|显示查询按钮|
|condition.updateOnChange|Boolean|false|查询条件改变立即更新|
|condition.toUrl|Boolean|true|将条件参数附加到浏览器 url|
|condition.list|Array[tablex.Model]|[]|查询列表|
|operation|Object|-|列表操作|
|operation.title|String|null|操作栏显示的标题文本|
|operation.width|String|null|操作栏的宽度，需要带 px 或 %|
|operation.toExpand|Boolean|true|当存在展开行时将操作加入到行内|
|operation.list|Array[tablex.Operation]|[]|操作列表|
|paging|Object|-|分页|
|paging.visible|Boolean|true|显示分页|
|paging.describe|Boolean|true|显示描述|
|paging.toUrl|Boolean|true|将分页参数附加到浏览器 url|
|paging.paramsFormat|Function|null|分页参数格式化回调|
|props|Object|-|额外参数|
|props.table|Object|-|同 el-table 组件参数|
|props.table.stripe|Boolean|true|是否为斑马纹 table|
|props.pagination|Object|-|同 el-pagination 组件参数|
|props.pagination.background|Boolean|true|是否为分页按钮添加背景色|
|props.pagination.small|Boolean|true|是否使用小型分页样式|
|props.pagination.pageSize|Number|10|每页显示条目个数|
|props.pagination.currentPage|Number|1|当前页数|
|props.pagination.pageSizes|Array[Number]|[5, 10, 20, 30, 40, 50, 100]|每页显示个数选择器的选项设置|

##### tablex.Model 模型配置

> 目前 model 、 condition.list 这两个组件配置项使用此模型配置，其中 model又分为 Visible（展示） 模式 与 Edit（编辑） 模式，但不是所有配置都适用。为方便阅读理解，在下面列表的适用配置项会用 model 、 condition 表示：

<table>
    <theader>
        <tr>
            <th>配置名</th>
            <th>类型</th>
            <th>默认值</th>
            <th>适用配置项</th>
            <th>适用 model 模式</th>
            <th>说明</th>
        </tr>
    </theader>
    <tbody>
        <tr>
            <td>type</td>
            <td>String</td>
            <td>tablex.String</td>
            <td>所有</td>
            <td>所有</td>
            <td>对应 tablex 数据类型<br>此处额外多两个类型：<br>$index：返回行号<br>expand：展开行，同 el-table 展开行功能，只适用于 Visible 模式。注意：一个数据模型里最多只能存在一个 expand</td>
        </tr>
        <tr>
            <td>key</td>
            <td>String</td>
            <td>-</td>
            <td>condition</td>
            <td>-</td>
            <td>数据键</td>
        </tr>
        <tr>
            <td>keyAlias</td>
            <td>String/Array[String]</td>
            <td>null</td>
            <td>所有</td>
            <td>所有</td>
            <td>数据键别名</td>
        </tr>
        <tr>
            <td>data</td>
            <td>Array[Any]</td>
            <td>-</td>
            <td>所有</td>
            <td>所有</td>
            <td>数据来源，等同于 tablex.type 构造函数的传参, 从第二位开始</td>
        </tr>
        <tr>
            <td>label</td>
            <td>String</td>
            <td>-</td>
            <td>所有</td>
            <td>所有</td>
            <td>组件显示的标签</td>
        </tr>
        <tr>
            <td>slots</td>
            <td>Function</td>
            <td>null</td>
            <td>所有</td>
            <td>所有</td>
            <td>插槽自定义函数</td>
        </tr>
        <tr>
            <td>format</td>
            <td>Function</td>
            <td>null</td>
            <td>所有</td>
            <td>所有</td>
            <td>格式化数据回调</td>
        </tr>
        <tr>
            <td>props</td>
            <td>Object</td>
            <td>-</td>
            <td>所有</td>
            <td>所有</td>
            <td>同对应的数据类型所对应的 element-ui 组件</td>
        </tr>
        <tr>
            <td>visible</td>
            <td>Boolean</td>
            <td>true</td>
            <td>所有</td>
            <td>Visible</td>
            <td>是否显示</td>
        </tr>
        <tr>
            <td>model</td>
            <td>Object(tablex.Model)</td>
            <td>null</td>
            <td>model</td>
            <td>Visible</td>
            <td>在 type 为 expand 时适用，格式同 tablex 组件配置的 model</td>
        </tr>
        <tr>
            <td>editOpts</td>
            <td>Object</td>
            <td>-</td>
            <td>所有</td>
            <td>Edit</td>
            <td>编辑/输入时的选项</td>
        </tr>
        <tr>
            <td>editOpts.vibible</td>
            <td>Boolean</td>
            <td>true</td>
            <td>所有</td>
            <td>Edit</td>
            <td>是否显示</td>
        </tr>
        <tr>
            <td>editOpts.readonly</td>
            <td>Boolean</td>
            <td>false</td>
            <td>所有</td>
            <td>Edit</td>
            <td>是否只读</td>
        </tr>
        <tr>
            <td>editOpts.watch</td>
            <td>Function</td>
            <td>null</td>
            <td>所有</td>
            <td>Edit</td>
            <td>值监听回调函数</td>
        </tr>
        <tr>
            <td>updateOnChange</td>
            <td>Boolean</td>
            <td>false</td>
            <td>condition</td>
            <td>-</td>
            <td>此查询条件值改变后立即请求更新</td>
        </tr>
    </tbody>
</table>

* 示例

```html
<template>
    <el-tablex :options="options"></el-tablex>
</template>
<script>
export default {
    computed: {
        options(){
            return {
                // model 示例
                model: {
                    // 使用 type 为 $index 显示行号
                    $index: {
                        type: '$index',
                    },
                    // 此处 name 对应 配置里的 key
                    name: {
                        // type 默认为 tablex.String,
                        type: 'String',
                        label: '姓名',
                    },
                    sex: {
                        type: 'Radio',
                        label: '性别',
                        // 由于性别需要单选，这里传入数据来源
                        data: [
                            // 第一个参数，性别列表，对应 tablex.Raido 构造函数的 options 参数
                            [{
                                label: '男',
                                // 注意此处 value 值必须为 String 类型
                                value: '0',
                            }, {
                                label: '女',
                                value: '1',
                            }],
                        ],
                    },
                    // 使用展开行，因为 expand 在一个数据模型里最多只能存在一个，所以可以直接使用 key 作为 type 类型
                    expand: {
                        model: {
                            age: {
                                label: '年龄',
                            },
                            address: {
                                label: '地址',
                                props: {
                                    // 使用多行文本框
                                    type: 'textarea',
                                },
                            },
                        },
                    },
                },
                // condition 查询条件示例
                condition: {
                    list: [{
                        key: 'name',
                        label: '姓名',
                    }],
                },
            };
        },
    },
};
</script>
```

##### tablex.Operation 配置

|配置名|类型|默认值|说明|
|:-|:-|:-|:-|
|key|String|-|操作唯一的键，可以通过 operation-${key} 使用插槽|
|label|String|-|操作显示的文本内容，使用插槽时无效|
|trigger|Function|-|点击时触发的回调。使用插槽时无效，需自定义交互|

* 示例

```html
<template>
    <el-tablex :options="options">
        <!-- 使用插槽 -->
        <el-button slot="operation-delete" slot-scope="scope" @click="deleteRow(scope)" type="danger" icon="el-icon-delete-solid" circle></el-button>
    </el-tablex>
</template>
<script>
export default {
    computed: {
        options() {
            return {
                operation: {
                    list: [{
                        key: 'edit',
                        label: '修改',
                        trigger: scope => {
                            // 通过 scope.row 可以获取当前操作的数据
                        },
                    }， {
                        // 使用插槽
                        key: 'delete',
                    }],
                },
            };
        },
    },
    methods: {
        deleteRow(scope) {
            // 通过 scope.row 可以获取当前操作的数据
        },
    },
};
</script>
```

----

##### tablex.Action 配置

> tablex 所有的交互都是围绕 action 开展的，通过 action 驱动增删改查及各种操作
> tablex 内置了四种常用的 action ：

* select （查询）select 是 tablex 表格的数据来源
* insert （新增）
* edit （修改）
* delete （删除）

|配置名|类型|默认值|说明|
|:-|:-|:-|:-|
|method|String/Function|-|此行为调用的服务端接口或自定义回调函数，必要参数，不填报错|
|params|Object|null|调用时附带的参数|
|before|Function|-|调用前请求参数格式化回调|
|after|Function|-|调用后响应数据格式化回调|
|editor|Object|-|表单弹窗配置|
|editor.customClass|String|-|自定义弹窗类名|
|editor.title|String|-|弹窗标题|
|editor.width|String|-|弹窗宽度，px 或 %|
|editor.props|Object|-|弹窗组件配置，同 el-dialog|
|editor.labelWidth|String|-|表单标签宽度，px 或 %|
|editor.model|Object(tablex.Model)|-|表单数据模型|
|editor.sendParams|Array[String]|-|定义发送参数列表|
|table|Object|-|表格弹窗配置|
|table.customClass|String|-|自定义弹窗类名|
|table.title|String|-|弹窗标题|
|table.width|String|-|弹窗宽度，px 或 %|
|table.props|Object|-|弹窗组件配置，同 el-dialog|
|table.options|Object(tablex.Options)|-|tablex 配置选项|
|table.on|Object|-|事件绑定|

* 示例

```html
<template>
    <el-tablex :options="options"></el-tablex>
</template>
<script>
import axios from 'axios';

export default {
    computed: {
        options() {
            return {
                operation: {
                    list: [{
                        // 更新状态操作按钮
                        key: 'status',
                        trigger: scope => scope.action('updateStatus', scope, scope.row.id),
                    }],
                },
                actions: {
                    // 查询数据
                    select: {
                        // 此接口为注册 tablex 时由全局配置 request.map 传入
                        method: 'getData',
                    },
                    // 自定义 action
                    // 更新状态
                    updateStatus: {
                        // 使用自定义回调
                        method: async (params, scope) => {
                            // 自定义请求
                            const res = await axios.post('http://XXXX/updateStatus', {
                                params,
                            });
                            // 响应数据必须返回
                            return res;
                        },
                    },
                },
            };
        },
    },
};
</script>
```

----
