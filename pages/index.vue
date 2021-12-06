<template>
  <div>
    <Navbar />
    <Table :type="available"/>

    <div class="footer">
      <el-button 
        type="warning"
        @click="$router.push('/example')"
        >
          Ir a ejemplo 2
      </el-button>
    </div>
  </div>
</template>

<style >
  .footer {
    position: absolute;
    bottom: 30px;
    width: 100%;
  }
</style>