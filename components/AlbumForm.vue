<template>
  <el-card
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
          <el-select v-model="form.state" placeholder="Selecciona" clearable>
            <el-option
              v-for="(state, i) in stateOptions"
              :key="state.value + i"
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
          <el-select v-model="form.review" placeholder="Selecciona" clearable>
            <el-option
              v-for="(review, i) in reviewOptions"
              :key="review.value + i"
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
          <el-input v-model="form.year" type="number" clearable> </el-input>
        </el-descriptions-item>
      </el-descriptions>
      <el-form-item>
            <el-button type="warning" @click="addAlbum()"
              >Añadir Album</el-button>
          </el-form-item>
      </el-form>
  </el-card>
</template>

<script>
import { Client } from '@notionhq/client'

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
        value: {
          id: "82fa8d86-20c8-4abe-898e-cabf14bd0ae4",
          name: "Parcialmente",
          color: "orange"
        }
      },
      {
        label: 'Escuchado',
        value: {
          id: "84d15b21-396d-4c6a-8cab-de88de521ac1",
          name: "Escuchado",
          color: "yellow"
        }
      },
      {
        label: 'Relisten',
        value: {
          id: "f8bd4651-92e9-4552-8c5d-abaa2aad0a9a",
          name: "Relisten",
          color: "red"
        }
      },
      {
        label: 'Sin escuchar',
        value: {
          id: "497cdac8-5e84-4e3c-b7ac-fbac5e696047",
          name: "Sin escuchar",
          color: "blue"
        }
      }],
      reviewOptions: [{
        label: '-',
        value: {
          id: "0c19afeb-7e77-4e18-8b82-f4efe3073022",
          name: "-",
          color: "default"
        }
      },
      {
        label: '⭐',
        value: {
          id: "4e80f68f-0b9c-4e28-b779-bedf3224bc7a",
          name: "⭐",
          color: "yellow"
        }
      },
      {
        label: '⭐⭐',
        value: {
          id: "9ee55d5d-dbef-424b-9bd1-b26cde9f75c0",
          name: "⭐⭐",
          color: "yellow"
        }
      },
      {
        label: '⭐⭐⭐',
        value: {
          id: "33ec82ce-4f08-4091-be93-b70b20119c93",
          name: "⭐⭐⭐",
          color: "yellow"
        }
      },
      {
        label: '⭐⭐⭐⭐',
        value: {
          id: "e0a88c7e-05b9-4ec4-80b8-e71342ba284d",
          name: "⭐⭐⭐⭐",
          color: "yellow"
        }
      },
      {
        label: '⭐⭐⭐⭐⭐',
        value: {
          id: "9ed6df6e-5622-4728-b7a2-54ed68e28b5f",
          name: "⭐⭐⭐⭐⭐",
          color: "yellow"
        }
      }],
      errors: [],
    }
  },
  methods: {
    async addAlbum() {
      const notion = new Client({ auth: process.env.secret_mtVR9yF1WxuaVYOfLbCZtFxPk9U9bYdiGMxrMf3Ruhc})
      const albumData = {
        "patent": {
          database_id: "c8441456a6f748cb915c502522cb9120"
        },
        "properties": {
          "Titulo": {
            "title": [
              {
                "text": {
                  "content": this.form.title
                }
              }
            ]
          },
          "Artista": {
            "text": [
              {
                "type": "text",
                "text": {
                  "content": this.form.artist,
                  "link": null
                },
                "annotations": {
                  "bold": false,
                  "italic": false,
                  "strikethrough": false,
                  "underline": false,
                  "code": false,
                  color: "default"
                },
                "plain_text": this.form.artist,
                "href": null
              }
            ]
          },
          "Estado": {
            "select": this.form.state
          },
          "Review": {
            "select": this.form.review
          },
          "Año": {
            "text": [
              {
                "type": "text",
                "text": {
                  "content": this.form.year,
                  "link": null
                },
                "annotations": {
                  "bold": false,
                  "italic": false,
                  "strikethrough": false,
                  "underline": false,
                  "code": false,
                  color: "default"
                },
                "plain_text": this.form.year,
                "href": null
              }
            ]
          },
        }
      }

      const response = await notion.pages.create(albumData)
      console.log(response)

      const config = {
        method: 'post',
        url: "https://api.notion.com/v1/pages",
        headers: {
          'Authorization': 'Bearer secret_mtVR9yF1WxuaVYOfLbCZtFxPk9U9bYdiGMxrMf3Ruhc',
          'Content-Type': 'application/json',
          'Notion-Version': '2021-05-13'
        },
        data: albumData
      }

      console.log(config)
    }
  }
}
</script>
