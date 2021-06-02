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
|request.axios.defaults.headers|Object|{ 'Content-Type': 'application/json;charset=UTF-8' }|请求头配置|
|request.axios.interceptors|Object|-|自定义拦截处理回调|
|request.axios.interceptors.request|Array[Function]|[]|请求处理回调列表|
|request.axios.interceptors.response|Array[Function]|[]|响应处理回调列表|
|table|Objecr|-|Table 组件配置|
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

* 例子

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

> 为了实现各种数据类型在界面上的展示、编辑，在此定义了几种数据类型

##### Type

基类，所有类型均继承此类。目前有以下类：

|类型|继承于|说明|
|:-|:-|:-|
|String|Type|字符串|
|JSON|Type|json 对象，尚未实现|
|Double|Type|浮点数|
|Password|String|密码|
|Date|Type|日期，YYYY-MM-DD HH:mm:ss|
|Time|Date|时间点，HH:mm:ss|
|DateRange|Type|时间区间，YYYY-MM-DD HH:mm:ss - YYYY-MM-DD HH:mm:ss|
|Select|Type|下拉选择器|
|Radio|Select|单选|
|Checkbox|Radio|多选|

###### Type 类实例有以下函数：

> 不同类型函数传参有所不同

* constructor 构造函数 & set

|参数|类型|默认值|适用类型|参数顺序|说明|
|:-|:-|:-|:-|:-|:-|
|value|Any|-|所有|-|原始数据值，不同类型会根据 value 作出不同的显示方式|
|formatString|String|YYYY-MM-DD HH:mm:ss|Date|value, formatString|格式化显示方式，不包含编辑状态|



* getEditor

> 获取类型编辑器，编辑器可修改数据值返回到数据源中。多用于表单提交新增、修改功能

|参数|类型|默认值|适用类型|参数顺序|说明|
|:-|:-|:-|:-|:-|:-|
|vue|Vue|-|所有|-|vue 实例对象|

* format

> 格式化数据值，当前无处理

----


####  tablex 组件

```html
<template>
    <!-- 引用 -->
    <el-talbex :options="options"></el-talbex>
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

----






