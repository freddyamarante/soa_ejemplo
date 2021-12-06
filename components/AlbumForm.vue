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
            Titulo
          </template>
          <el-input v-model="form.title" clearable> </el-input>
        </el-descriptions-item>
        <el-descriptions-item :span="3">
          <template slot="label">
            <i class="el-icon-postcard"></i>
            Artista
          </template>
          <el-input v-model="form.artist" clearable> </el-input>
        </el-descriptions-item>
          <el-descriptions-item :span="3">
          <template slot="label">
            <i class="el-icon-user"></i>
            Estado
          </template>
          <el-select v-model="form.state" placeholder="Selecciona"  clearable>
            <el-option
              v-for="state in stateOptions"
              :key="state.value"
              :label="`${state.label}`"
              :value="state.value"
            >
            </el-option>
          </el-select>
        </el-descriptions-item>
          <el-descriptions-item :span="3">
          <template slot="label">
            <i class="el-icon-user"></i>
            Review
          </template>
          <el-select v-model="form.revuew" placeholder="Selecciona"  clearable>
            <el-option
              v-for="review in reviewOptions"
              :key="review.value"
              :label="`${review.label}`"
              :value="review.value"
            >
            </el-option>
          </el-select>
        </el-descriptions-item>
        <el-descriptions-item :span="3">
          <template slot="label">
            <i class="el-icon-postcard"></i>
            Año
          </template>
          <el-input v-model="form.year" clearable> </el-input>
        </el-descriptions-item>
      </el-descriptions>
      <el-form-item>
            <el-button type="warning" @click="addPage()"
              >Añadir Pagina</el-button>
          </el-form-item>
      </el-form>
  </el-card>
</template>

<script>
export default {
  name: 'AlbumForm',
  data() {
    return {
      form: {
        title: '', // title - string
        artist: '', // text - string
        state: '', // select - string
        review: '', // select - string
        year: '', // text - number
      }, 
      stateOptions: [{
        label: 'Parcialmente',
        value: 'Parcialmente'
      },
      {
        label: 'Escuchado',
        value: 'Escuchado'
      },
      {
        label: 'Relisten',
        value: 'Relisten'
      },
      {
        label: 'Sin escuchar',
        value: 'Sin escuchar'
      }],
      reviewOptions: [{
        label: '-',
        value: '-'
      },
      {
        label: '⭐',
        value: '⭐'
      },
      {
        label: '⭐⭐',
        value: '⭐⭐'
      },
      {
        label: '⭐⭐⭐',
        value: '⭐⭐⭐'
      },
      {
        label: '⭐⭐⭐⭐',
        value: '⭐⭐⭐⭐'
      },
      {
        label: '⭐⭐⭐⭐⭐',
        value: '⭐⭐⭐⭐⭐'
      }],
      errors: [],
    }
  }
}
</script>
