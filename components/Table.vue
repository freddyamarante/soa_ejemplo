<template>
  <div>
    <h1 v-if="type === 'available'">
      Páginas en el presente workspace
    </h1>
    <h1 v-if="type === 'deleted'">
      Páginas eliminadas
    </h1>
    <el-table
      :data="
        tableData.filter(
          (data) =>
            !search ||
            data.page_name.toLowerCase().includes(search.toLowerCase())
        )
      "
      empty-text="No hay paginas"
      style="width: 100%"
    >
      <el-table-column label="ID" prop="id"> </el-table-column>
      <el-table-column label="Icono" prop="icon"> </el-table-column>
      <el-table-column label="Pagina" prop="page_name"> </el-table-column>
      <el-table-column label="Tipo de contenido" prop="type_content">
      </el-table-column>
      <el-table-column
        v-if="type === 'available'"
        label="Creado en"
        prop="created_at"
      >
      </el-table-column>
      <el-table-column
        v-if="type === 'deleted'"
        label="Eliminado en"
        prop="deleted_at"
      >
      </el-table-column>
      <el-table-column align="right">
        <template #header>
          <el-input
            v-model="search"
            size="mini"
            placeholder="Escribe para buscar"
          />
        </template>
        <template slot-scope="scope">
          <el-button
            v-if="type === 'available'"
            icon="el-icon-delete"
            size="medium"
            type="danger"
            circle
            @click="handleDelete(scope.row)"
          >
          </el-button>
          <el-button
            v-if="type === 'deleted'"
            icon="el-icon-back"
            size="medium"
            type="warning"
            circle
            @click="handleRestore(scope.row)"
          >
          </el-button>
          <el-button
            v-if="type === 'deleted'"
            icon="el-icon-delete-solid"
            size="medium"
            circle
            @click="handlePermanentDelete(scope.row)"
          >
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
      @click="$router.push('/addpage')"
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
      default: 'available',
    },
  },
  data() {
    return {
      tableData: [],
      search: '',
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
      this.$confirm(
        '¿Estás seguro que deseas restaurar esta página?',
        'Warning',
        {
          confirmButtonText: 'Si',
          cancelButtonText: 'No',
          type: 'warning',
        }
      ).then(() => {
        this.$axios
          .$post(`http://localhost:3333/pages/${row.id}/restore`)
          .then(() => {
            this.$message({
              type: 'success',
              message: 'Se ha restaurado correctamente',
            })
            this.tableData = this.tableData.filter((page) => page.id !== row.id)
          })
      })
    },

    handleDelete(row) {
      this.$confirm(
        '¿Estás seguro que deseas eliminar esta página?',
        'Warning',
        {
          confirmButtonText: 'Si',
          cancelButtonText: 'No',
          type: 'warning',
        }
      ).then(() => {
        this.$axios
          .$delete(`http://localhost:3333/pages/${row.id}`)
          .then(() => {
            this.$message({
              type: 'success',
              message: 'Se ha eliminado correctamente',
            })
            this.tableData = this.tableData.filter((page) => page.id !== row.id)
          })
      })
    },

    handlePermanentDelete(row) {
      this.$confirm(
        '¿Estás seguro que deseas eliminar esta página permanentemente?',
        'Warning',
        {
          confirmButtonText: 'Si',
          cancelButtonText: 'No',
          type: 'error',
        }
      ).then(() => {
        this.$axios.$delete(`http://localhost:3333/pages/permanent/${row.id}`)
          .then(() => {
            this.$message({
              type: 'success',
              message: 'Se ha eliminado correctamente',
            })
            this.tableData = this.tableData.filter((page) => page.id !== row.id)
          })
      })
    },

    async getPages() {
      this.tableData = await this.$axios.$get(`http://localhost:3333/pages`)
      this.tableData.createdAt = moment
        .unix(this.tableData.createdAt)
        .format('dd.MM.yyyy')
    },

    async getDeletedPages() {
      this.tableData = await this.$axios.$get(
        `http://localhost:3333/pages/deleted`
      )
      this.tableData.deletedAt = moment
        .unix(this.tableData.deletedAt)
        .format('dd.MM.yyyy')
    },
  },
}
</script>

<style>
.el-button-center {
  margin-top: 25px !important;
  margin: 0 auto;
  display: block;
}

.el-button {
  margin: auto;
}
</style>