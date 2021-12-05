/* eslint-disable no-unused-expressions */
/* eslint-disable no-labels */

<template>
  <el-card
    v-loading="loading"
    class="row-bg box-card"
    shadow="hover"
    :body-style="{ padding: '0px' }"
  >
    <el-form ref="form" :model="form">
      <el-descriptions border>
        <el-descriptions-item :span="3">
          <template slot="label">
            <i class="el-icon-postcard"></i>
            Nombre de la pagina
          </template>
          <el-input v-model="form.typeContent" clearable> </el-input>
        </el-descriptions-item>
        <el-descriptions-item :span="3">
          <template slot="label">
            <i class="el-icon-user"></i>
            Tipo de pagina
          </template>
          <el-select v-model="form.type" placeholder="Selecciona"  clearable>
            <el-option
              v-for="item in options"
              :key="item.value"
              :label="`${item.label}`"
              :value="item.value"
            >
            </el-option>
          </el-select>
        </el-descriptions-item>
      </el-descriptions>
      <el-form-item>
            <el-button v-if="type === 'create'" type="warning"
              >Añadir Pagina</el-button>
          </el-form-item>
      </el-form>
  </el-card>
</template>

<script>
export default {
  name: 'PageForm',
  data() {
    return {
      form: {

      },
      options: [{
        value: 'text',
        label: 'Pagina normal'
        }, {
        value: 'database',
        label: 'Base de datos'
      }],
      value: ''
    }
  },

}
</script>

<style>
.box-card {
  width: 750px;
}

.el-form {
  text-align: center;
}

.el-button {
  margin-top: 10px;
  height: 40;
}

.el-card {
  background: #fef9c7
}
</style>