<template>
  <el-menu mode="horizontal">
    <el-menu-item>{{ title }}</el-menu-item>
  </el-menu>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import { ElMenu, ElMenuItem } from "element-plus"
import { backend } from '../axios'

export default defineComponent({
  components: {
    ElMenu,
    ElMenuItem
  },
  data() {
    return {
      title: ''
    }
  },
  methods: {
    async loadTitle() {
      const resp = await backend.get<string>('/title')
      this.title = resp.data
    }
  },
  async mounted() {
    await this.loadTitle()
    document.title = this.title
  }
})
</script>
