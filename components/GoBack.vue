<template>
  <el-page-header @back="$router.go(-1)"></el-page-header>
</template>

<script>
export default {
  name: 'GoBack'
}
</script>