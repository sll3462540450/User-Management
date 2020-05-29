<template>
  <div class="edit container">
    <hr />
    <!-- 绑定Alert.vue 传来的message ,判断是否有值-->
    <Alert v-if="alert" v-bind:message="alert" />
    <h1>更新信息</h1>
    <form v-on:submit="updataCustomer">
      <div class="well">
        <div class="form-group">
          <label>姓名</label>
          <input class="form-control" type="text" v-model="customer.name" placeholder="name" />
        </div>
        <div class="form-group">
          <label>电话</label>
          <input class="form-control" type="text" v-model="customer.phone" placeholder="phone" />
        </div>
        <div class="form-group">
          <label>邮箱</label>
          <input class="form-control" type="text" v-model="customer.email" placeholder="email" />
        </div>
        <div class="form-group">
          <label>学历</label>
          <input
            class="form-control"
            type="text"
            v-model="customer.education"
            placeholder="education"
          />
        </div>
        <div class="form-group">
          <label>毕业学校</label>
          <input
            class="form-control"
            type="text"
            v-model="customer.graduationSchool"
            placeholder="graduationSchool"
          />
        </div>
        <div class="form-group">
          <label>职业</label>
          <input
            class="form-control"
            type="text"
            v-model="customer.profession"
            placeholder="profession"
          />
        </div>
        <div class="form-group">
          <label>个人简历</label>
          <textarea class="form-control" v-model="customer.profile" placeholder="profile" rows="10"></textarea>
        </div>
        <button class="btn btn-primary" type="submit">确认</button>
      </div>
    </form>
  </div>
</template>

<script>
import Alert from "./Alert";
export default {
  name: "Edit",
  components: {
    Alert
  },
  data() {
    return {
      customer: {},
      alert:''
    };
  },
  created() {
    fetch("http://localhost:3000/users/" + this.$route.params.id)
      .then(res => res.json())
      .then(data => {
        this.customer = data;
      });
  },
  methods: {
    updataCustomer: function(e) {
      if (!this.customer.name || !this.customer.phone || !this.customer.email) {
        this.alert = "请填写对应的信息";
      } else {
        let updataCustomer = {
          name: this.customer.name,
          phone: this.customer.phone,
          email: this.customer.email,
          education: this.customer.education,
          graduationSchool: this.customer.graduationSchool,
          profession: this.customer.profession,
          profile: this.customer.profile
        };
        fetch("http://localhost:3000/users/" + this.$route.params.id, {
          method: "PUT",
          body: JSON.stringify(updataCustomer),
          headers: new Headers({
            "Content-Type": "application/json"
          })
        })
          .then(res => res.json())
          .then(res =>
            this.$router.push({
              path: "/",
              query: { alert: "用户信息更新成功" }
            })
          );
        // this.$http.put('http://localhost:3000/users/'+ this.$route.params.id,updataCustomer)
        // .then(res=>{
        //      this.$router.push({
        //       path: "/",
        //       query: { alert: "用户信息更新成功" }
        //     });
        // })
        e.preventDefault();
      }
      e.preventDefault();
    }
  }
};
</script>
