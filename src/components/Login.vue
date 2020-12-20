<template>
  <div class="hello">
      <h1>{{ msg }}</h1>
      <div>
          <el-input v-model="userID" placeholder="用户名" class='input'></el-input>
      </div>
      <br>
      <div>
          <el-input v-model="password" placeholder="密码" class='input' type='password'></el-input>
      </div>
      <br>
      <div>
        <el-button type="primary" @click="cilckLogin()">登录</el-button>
      </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      msg: 'Sign in',
      userID: '',
      password: ''
    }
  },
  methods: {
    cilckLogin: function () {
      var that = this
      this.$axios.request({
        url: 'http://localhost:8081/user/login',
        method: 'Post',
        data: JSON.stringify({
          Username: this.userID,
          Password: this.password
        }),
        responseType: 'json'
      }).then(function (response) {
        console.log(response.data)
        if (!response.data.error) {
          that.$store.commit('saveToken', {
            username: that.userID,
            token: response.data.ok
          })
          that.$router.push('/')
        } else {
          alert(response.data.error)
        }
      })
    }
  }
}
</script>

<style scoped>
.input{
  width:300px
}
</style>
