<template>
  <el-row>
    <el-col :span="5">
      <slot name="name"></slot>
    </el-col>
    <el-col :span="19"> <slot></slot> </el-col>
  </el-row>
</template>
<script lang="ts">
import { defineComponent } from 'vue'
import { ElRow, ElCol } from "element-plus"

export default defineComponent({
  components: {
    ElRow, ElCol
  }
})
</script>
