<template>
  <el-container>
    <el-header>
      <Header></Header>
    </el-header>
    <el-main> <NodeList></NodeList></el-main>
  </el-container>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import Header from './components/Header.vue'
import NodeList from './components/NodeList.vue'
import { ElHeader, ElMain, ElContainer } from "element-plus"

export default defineComponent({
  components: {
    Header, NodeList, ElHeader, ElMain, ElContainer
  }
})
</script>

<style>
body {
  margin: 0;
}
</style>
