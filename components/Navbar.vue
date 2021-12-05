<template>
  <el-menu 
    :default-active="$route.path" 
    mode="horizontal"
    background-color="#fce181"
    text-color="#00000"
    active-text-color="#00000"
    >
    <el-menu-item  index="/" @click="$router.push('/')">Inicio</el-menu-item>
    <el-menu-item  index="/deleted" @click="$router.push('/deleted')">Borrados</el-menu-item>
  </el-menu>
</template>

<script>
  export default {
    name: 'Navbar',
    data() {
      return {
        activeName: 'first',
      }
    },
  }
</script>

<style scoped>
.el-menu {
  width:100%;
  color:black
}
</style>
