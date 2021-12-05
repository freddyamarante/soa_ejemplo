<template>
<div>
    <el-table
      :data="data"
      style="width: 100%"
    >
      <el-table-column label="ID" prop="id"> </el-table-column>
      <el-table-column label="Pagina" prop="page_name"> </el-table-column>
      <el-table-column label="Tipo de contenido" prop="type_content"> </el-table-column>
      <el-table-column v-if="type === 'available'" label="Creado en" prop="created_at"> </el-table-column>
      <el-table-column v-if="type === 'deleted'" label="Eliminado en" prop="deleted_at"> </el-table-column>
      <el-table-column align="right">
        <template #header>
          <el-input v-model="search" size="mini" placeholder="Escribe para buscar" />
        </template>
        <template slot-scope="scope">
          <el-button
            v-if="type === 'available'"
            size="mini"
            type="danger"
            @click="handleDelete(scope.row)"
            >
            Borrar
            </el-button>
          <el-button
            v-if="type === 'deleted'"
            size="mini"
            type="warning"
            @click="handleRestore(scope.row)"
            >
            Restaurar
            </el-button>
        </template>
      </el-table-column>
    </el-table>
    <el-button
        v-if="type === 'available'"
        type="info"
        class="el-button-center"
        icon="el-icon-plus"
        circle
        @click="$router.push('')"
      >
      </el-button>
    </div>
</template>

<script>
import moment from 'moment'

export default {
  name: 'Table',
  props: {
    type: {
      type: String,
      default: 'available'
    }
  },
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
    if (this.type === 'available') {
      this.getPages()
    } else {
      this.getDeletedPages()
    }
  },
  methods: {
    handleRestore(row) {
      this.$axios.$post(`http://localhost:3333/pages/${row.id}/restore`)
    },

    handleDelete(row) { 
      this.$axios.$delete(`http://localhost:3333/pages/${row.id}`)
    },

    async getPages() {
      this.tableData = await this.$axios.$get(`http://localhost:3333/pages`)
      this.tableData.createdAt = moment(this.tableData.createdAt, 'dd.MM.yyyy')
    },

    async getDeletedPages() {
      this.tableData = await this.$axios.$get(`http://localhost:3333/pages/deleted`)
      this.tableData.createdAt = moment(this.tableData.createdAt, 'dd.MM.yyyy')
    }


  },
}
</script>

<style>
.el-button-center {
  margin-top: 50px !important;
  margin: 0 auto;
  display: block;
}

.el-button {
  margin: 0 auto;
  display:block
}
</style>