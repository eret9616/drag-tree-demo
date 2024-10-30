<!-- eslint-disable -->
<template>
  <div>
    <h3>元素结构</h3>
    <el-tree
      :data="treeData"
      :props="defaultProps"
      node-key="id"
      draggable
      @node-drop="handleNodeDrop"
      default-expand-all
      highlight-current
      style="border: 1px solid #ebeef5; padding: 10px"
    >
      <span slot-scope="{ node, data }">
        {{ data.label }}
      </span>
    </el-tree>
  </div>
</template>

<script>
/* eslint-disable */
export default {
  props: {
    treeData: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      defaultProps: {
        children: "children",
        label: "label",
      },
    };
  },
  methods: {
    handleNodeDrop(draggingNode, dropNode, dropType, ev) {
      // 拖拽完成后的处理逻辑（通常不需要额外处理）
    },
  },
};
</script>
