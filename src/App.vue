<template>
  <div id="app">
  <el-menu
    :router="true"
    :default-active="this.$route.path"
    class="header"
    mode="horizontal"
    @select="handleSelect"
    background-color="#545c64"
    text-color="#fff"
    active-text-color="#ffd04b">
    <el-menu-item index="/" >
      主页
    </el-menu-item>
    <el-menu-item index="/user" style='float:right' v-if="this.$store.state.username">
      {{ this.$store.state.username }}
    </el-menu-item>
    <div v-else="">
      <el-menu-item class = "log" index="/join" style='float:right'>注册</el-menu-item>
      <el-menu-item class = "log" index="/login" style='float:right'>登陆</el-menu-item>
    </div>
  </el-menu>
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
body {
  margin: 0;
}
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  text-align: center;
}
</style>
