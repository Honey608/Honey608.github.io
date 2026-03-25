+++
title = "Vue中的$emit $on $once $off的使用"
date = 2019-08-23

[taxonomies]
tags = ["web"]
+++
<!-- more -->
1.如果对以上方法还不了解的可以点击[传送门](https:

> 最近的大事就是香港暴乱,做好自己就是对祖国的最好支持！
//cn.vuejs.org/v2/api/#vm-on)大致了解一下😯
2.都说实践出真知，复制以下代码，动动小手点击下就全懂啦！
```javascript
<body>
    <div id="app">
        <p>{{msg}}{{count}}</p>
        <children-component @aa="aa"></children-component> <br/>
        <button @click="removeClick">父组件按钮使用$off移除$on监听</button>
    </div>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script>
        var bus = new Vue()
        var children = {
                data(){
                    return{
                        data:""
                    }
                },
                methods: {
                    childrenClick(){
                        bus.$emit('aa','子组件通过emit发送过来的数据')
                    },
                    hanldClick(){
                        bus.$emit('bb','子组件用来探索$once')
                    }
                },
            template:`<div>
                <button @click="childrenClick">子组件使用$emit发送数据，父组件使用$on监听</button><br/><br/> 
                <button @click="hanldClick">子组件使用$emit发送数据，父组件使用$once监听只能触发一次</button>
            </div>`
        }
        var vm = new Vue({
            el:"#app",
            data:{
                msg:'',
                count:0
            },
            components:{
                'children-component':children
            },
            methods:{
                aa(s){
                    console.log(s)  
                },
                removeClick(){
                    bus.$off('aa')
                }
            },
            mounted(){
                bus.$on('aa',(val)=>{
                    this.msg = val
                    this.count++
                })
                bus.$once('bb',(val)=>{
                    console.log(val)
                })
            }
        })
    </script>
</body>
```

