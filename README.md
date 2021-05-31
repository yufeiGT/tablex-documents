#### tablex

> tablex 是一个基于 [Vue](https://cn.vuejs.org/) 、[ELement UI](https://element.eleme.cn/#/zh-CN) 、 [Axios](http://www.axios-js.com/) 实现的一个 table 组件，可根据配置快速实现增删改查业务

----

##### 安装

```javascript
npm i @~crazy/merge -S
```

----

##### 使用

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

##### tablex 全局属性

|属性名|说明|
|:-|:-|
|version|版本号|
|api|接口对象，根据 request.map 配置生成的接口函数|
|responseFormat|全局请求响应数据格式化回调|

----

##### 全局配置

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

##### request.map 接口地图

----
