<template>
  <div class="nodes-container">
    <NodeDetail
      v-for="node in nodes"
      :key="node.id"
      :node="node"
      :nodeStatus="nodeStatusMap.get(node.id)"
    ></NodeDetail>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'
import { backend } from '../axios'
import { NodeStatus, Node } from './NodeDetail.vue'
import NodeDetail from './NodeDetail.vue'

export default defineComponent({
  components: {
    NodeDetail
  },
  data() {
    return {
      nodes: new Array<Node>(),
      nodeStatusMap: new Map<string, NodeStatus>()
    }
  },
  methods: {
    async loadNodes() {
      const resp = await backend.get<Node[]>('/nodes')
      this.nodes = resp.data
    },
    async loadNodeStatus() {
      for (const node of this.nodes) {
        if (node.is_alive) {
          const resp = await backend.get<NodeStatus>('/nodes/' + node.id + '/status')
          const status = resp.data
          status.cpu = Math.round(status.cpu ?? 0)
          status.mem.percent = Math.round(status.mem.percent ?? 0)
          status.swap.percent = Math.round(status.swap.percent ?? 0)
          this.nodeStatusMap.set(node.id, resp.data)
        }
      }
    }
  },
  async mounted() {
    await this.loadNodes()
    await this.loadNodeStatus()
    setInterval(this.loadNodeStatus, 3000)
  }
})
</script>

<style scoped>
.nodes-container {
  display: grid;
  grid-template-columns: repeat(auto-fit, 380px);
  justify-content: center;

  gap: 20px;
}
</style>