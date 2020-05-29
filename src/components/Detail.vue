<template>
  <div class="detail container">
    <router-link to="/" class="btn btn-default">返回</router-link>
    <h1 class="page-header">
      {{customer.name}}
      <!-- 编辑和删除按钮 -->

      <span class="pull-right">
        <router-link v-bind:to="'/edit/'+customer.id" class="btn btn-primary">编辑</router-link>
        <button class="btn btn-danger" @click="deleteCustomer(customer.id)">删除</button>
      </span>
    </h1>

    <ul class="list-group">
      <li class="list-group-item">
        <span class="glyphicon glyphicon-user">{{customer.phone}}</span>
      </li>
      <li class="list-group-item">
        <span class="glyphicon glyphicon-envelope">{{customer.email}}</span>
      </li>
    </ul>
    <ul class="list-group">
      <li class="list-group-item">
        <span class="glyphicon glyphicon-th">{{customer.education}}</span>
      </li>
      <li class="list-group-item">
        <span class="glyphicon glyphicon-home">{{customer.graduationSchool}}</span>
      </li>
      <li class="list-group-item">
        <span class="glyphicon glyphicon-asterisk">{{customer.profession}}</span>
      </li>
      <li class="list-group-item">
        <span class="glyphicon glyphicon-file">{{customer.profile}}</span>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "detail",
  data() {
    return {
      customer: ""
    };
  },
  methods: {
    fetchCustomers(id) {
      fetch("http://localhost:3000/users/" + id)
        .then(res => res.json())
        .then(res => {
          this.customer = res;
          this.$route.params.id;
          console.log(res);
        });
      // this.$http.get("http://localhost:3000/users/" + id).then(res => {
      //   this.customer = res.body;
      //   console.log(res);
      // });
    },
    deleteCustomer: function(id) {
      fetch("http://localhost:3000/users/" + id, {
        method: "delete"
      })
        .then(res => res.json())
        .then(data => {
          this.$router.push({ path: "/", query: { alert: "用户删除成功" } });
        });
    }
  },
  created() {
    this.fetchCustomers(this.$route.params.id);
  }
};
</script>
