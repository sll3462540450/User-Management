<template>
  <div class="customers container">
    <Alert v-if="alert" v-bind:message="alert" />
    <h1>用户管理系统</h1>
    <input type="text" class="form-control" placeholder="搜索" v-model="filterInput" />
    <table class="table table-striped">
      <thead>
        <tr>
          <th>姓名</th>
          <th>电话</th>
          <th>邮箱</th>
        </tr>
      </thead>
      <tbody>
        <!-- 传入一个 filterBy()方法，-->
        <tr v-for="customer in filterBy(customers,filterInput)" :key="customer.id">
          <td>{{customer.name}}</td>
          <td>{{customer.phone}}</td>
          <td>{{customer.email}}</td>
          <td>
            <router-link class="btn btn-default" v-bind:to="'/customer/'+customer.id">详情</router-link>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import Alert from "./Alert";
export default {
  name: "customers",
  components: { Alert },

  data() {
    return {
      customers: [],
      alert: "",
      filterInput: ""
    };
  },
  mounted() {
    // 判断alert有没有内容
    if (this.$route.query.alert) {
      this.alert = this.$route.query.alert;
    }
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
