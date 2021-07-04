<template>
  <el-card>
    <template #header>
      <div class="card-header">
        <span>{{ node?.id }}</span>
        <span v-if="node?.is_alive" class="online-text">在线</span>
        <span v-else class="offline-text">离线</span>
      </div>
    </template>
    <StatusItem class="status-item">
      <template #name><span>CPU</span></template>
      <el-progress
        :text-inside="true"
        :percentage="nodeStatus?.cpu"
        :stroke-width="20"
      ></el-progress>
    </StatusItem>

    <StatusItem class="status-item">
      <template #name><span>RAM</span></template>
      <el-progress
        :text-inside="true"
        :percentage="nodeStatus?.mem.percent"
        :stroke-width="20"
      >
      </el-progress>
    </StatusItem>
    <StatusItem class="status-item">
      <span>{{
        formatBytes(nodeStatus?.mem.used_mem ?? 0) +
        "/" +
        formatBytes(nodeStatus?.mem.total_mem ?? 0)
      }}</span>
    </StatusItem>

    <StatusItem class="status-item">
      <template #name><span>SWAP</span></template>
      <el-progress
        :text-inside="true"
        :percentage="nodeStatus?.swap.percent"
        :stroke-width="20"
      ></el-progress>
    </StatusItem>
    <StatusItem class="status-item">
      <span>{{
        formatBytes(nodeStatus?.swap.used_mem ?? 0) +
        "/" +
        formatBytes(nodeStatus?.swap.total_mem ?? 0)
      }}</span>
    </StatusItem>

    <StatusItem class="status-item">
      <template #name><span>Uptime</span></template>
      <span>{{ formatUptime(nodeStatus?.uptime ?? 0) }}</span>
    </StatusItem>

    <StatusItem class="status-item">
      <template #name> <span>Net</span></template>
      <el-row>
        <el-col :span="12">
          <span class="el-icon-download">{{
            formatBytes(nodeStatus?.net.rx ?? 0) + "/s"
          }}</span></el-col
        >
        <el-col :span="12"
          ><span class="el-icon-upload2">{{
            formatBytes(nodeStatus?.net.tx ?? 0) + "/s"
          }}</span>
        </el-col>
      </el-row>
    </StatusItem>
  </el-card>
</template>

<script lang="ts">
import { Duration } from 'luxon'
import { defineComponent, PropType } from 'vue'
import { ElCard, ElRow, ElCol, ElProgress } from "element-plus"
import StatusItem from "./StatusItem.vue"

export interface NodeStatus {
  cpu: number
  mem: {
    total_mem: number
    used_mem: number
    percent: number
  }
  swap: {
    total_mem: number
    used_mem: number
    percent: number
  }
  uptime: number
  net: {
    rx: number
    tx: number
  }
}

export interface Node {
  id: string
  is_alive: boolean
}

export default defineComponent({
  props: {
    node: {
      type: Object as PropType<Node>
    },
    nodeStatus: {
      type: Object as PropType<NodeStatus>
    }
  },

  components: {
    ElCard, ElRow, ElCol, ElProgress, StatusItem
  },
  methods: {
    formatBytes(size: number) {
      if (size < (1024 * 1024)) {
        let temp = size / 1024
        return temp.toFixed(2) + 'KB'
      }
      else if (size < (1024 * 1024 * 1024)) {
        let temp = size / (1024 * 1024)
        return temp.toFixed(2) + 'MB'
      } else {
        let temp = size / (1024 * 1024 * 1024)
        return temp.toFixed(2) + 'GB'
      }
    },
    formatUptime(uptime: number): string {
      const du = Duration.fromObject({
        seconds: uptime,
      })
      return du.toFormat('d天h小时m分钟')
    },
  }
})
</script>
<style scoped>
.card-header {
  display: flex;
  justify-content: space-between;
}
.online-text {
  color: #67c23a;
}
.offline-text {
  color: #f56c6c;
}
.status-item {
  margin-bottom: 10px;
}
</style>