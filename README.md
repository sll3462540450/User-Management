# students

> A Vue.js project

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).


首先我用的是本地服务器数据：如何搭建本地服务器，以下是我的简书可以参考
https://www.jianshu.com/p/ef35929506a6


本项目中用到BootStrap：需要引入Bootstrap包
引入路由  https://router.vuejs.org/zh/installation.html
```
npm install vue-router
```
```
import Vue from 'vue'
import VueRouter from 'vue-router'

Vue.use(VueRouter)
```

`vue-resource`是vue中使用的请求网络数据的插件，这个插件是依赖于vue的，简单说就是用来调接口的。
不需要--save 也可以
```
npm i vue vue-resource --save-dev
```
```
import VueResource from 'vue-resource'
Vue.use(VueResource)
```


设置路由
```
// 设置路由
const router = new VueRouter({
  mode: 'history',
  base: __dirname,
  routes: [{
      path: '/',
      component: Customers
    }
    .......
  ]
})
```
```
new Vue({
  router,
   template:
   `
    导航栏模板
   `
}).$mount('#app')
```

# 获取后台数据
实现搜索功能：`filterInput `传入方法,match() 此方法的返回值是存放首次匹配成功内容的数组，匹配失败，返回 null
```
<tr v-for="customer in filterBy(customers,filterInput)" :key="customer.id">
          <td>{{customer.name}}</td>
          <td>{{customer.phone}}</td>
          <td>{{customer.email}}</td>
          <td>
            <router-link class="btn btn-default" v-bind:to="'/customer/'+customer.id">详情</router-link>
          </td>
  </tr>
  <script>
  export default {
  name: "customers",
  data(){
    return {
      customers: [],
     
      filterInput: ""
    };
  },
    mounted() {
    fetch("http://localhost:3000/users")
      .then(res => res.json())
      .then(data => (this.customers = data));
  },
   methods: {
    // match() 此方法的返回值是存放首次匹配成功内容的数组，匹配失败，返回 null
    filterBy: function(customers, value) {
      return customers.filter(customer => {return customer.name.match(value)});
    }
  }
  };
  </script>
```

# 添加数据
点击 `addCustomer`按钮进行添加数据到首页
```
  <form v-on:submit="addCustomer">
    <div class="well">
       <div class="form-group">
          <label>姓名</label>
          <input class="form-control" type="text" v-model="customer.name" placeholder="name" />
        </div>
        ......
        ......
        ......
    </div> 
  </form>
  
  
  export default {
  name: "add",
   data() {
    return {
      customer: {},
    };
  },
  methods: {
    addCustomer: function(e) {
      if (!this.customer.name || !this.customer.phone || !this.customer.email) {
        this.alert = "请填写对应的信息";
      } else {
        let newCustomer = {
          name: this.customer.name,
          phone: this.customer.phone,
          email: this.customer.email,
          education: this.customer.education,
          graduationSchool: this.customer.graduationSchool,
          profession: this.customer.profession,
          profile: this.customer.profile
        };
        fetch("http://localhost:3000/users", {
          method: "POST",
          body: JSON.stringify(newCustomer),
          headers: new Headers({
            "Content-Type": "application/json"
          })
        })
          .then(res => res.json())
          .then(res =>
            this.$router.push({
              path: "/",
              query: { alert: "用户信息添加成功" }
            })
          );
        e.preventDefault();
      }
      e.preventDefault();
    }
  }
  };
</script>
```

# 编辑删除
点击按钮通过`id`跳转到 `edit` 组件中
```
 <span class="pull-right">
        <router-link v-bind:to="'/edit/'+customer.id" class="btn btn-primary">编辑</router-link>
        <button class="btn btn-danger" @click="deleteCustomer(customer.id)">删除</button>
 </span>
```
跳转到根目录就是首页并提示删除成功
```
this.$router.push({ path: "/", query: { alert: "用户删除成功" } });
```
