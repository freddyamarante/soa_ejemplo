<template>
  <el-table
    :data="data"
    style="width: 100%"
  >
    <el-table-column label="ID" prop="id"> </el-table-column>
    <el-table-column label="Pagina" prop="page_name"> </el-table-column>
    <el-table-column label="Tipo de contenido" prop="type_content"> </el-table-column>
    <el-table-column label="Creado en" prop="created_at"> </el-table-column>
    <el-table-column align="right">
      <template #header>
        <el-input v-model="search" size="mini" placeholder="Escribe para buscar" />
      </template>
      <template slot-scope="scope">
        <el-button size="mini" @click="handleEdit(scope.$index, scope.row)"
          >Editar</el-button
        >
        <el-button
          size="mini"
          type="danger"
          @click="handleDelete(scope.$index, scope.row)"
          >Borrar</el-button
        >
      </template>
    </el-table-column>
  </el-table>
</template>

<script>
import moment from 'moment'

export default {
  name: 'Table',
  data() {
    return {
      tableData: [],
      search: '',
    }
  },
  computed: {
    data() {
      return (this.tableData.filter(
        (data) =>
          !this.name_page || data.text.toLowerCase().includes(this.search.toLowerCase())
      ))
    }
  },
  mounted() {
    this.getPages()
  },
  methods: {
    handleEdit(index, row) {
      console.log(index, row)
    },

    handleDelete(index, row) { 
      alert(this.tableData.id)
      this.$axios.$delete(`http://localhost:3333/pages/${(index, row)}`)
    },

    async getPages() {
      this.tableData = await this.$axios.$get(`http://localhost:3333/pages`)
      this.tableData.createdAt = moment(this.tableData.createdAt, 'dd.MM.yyyy')
    }


  },
}
</script>


