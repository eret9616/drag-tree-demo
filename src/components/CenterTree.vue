<!-- eslint-disable -->
<template>
  <div>
    <h3>元素结构</h3>
    <el-tree
      :data="treeData"
      :props="defaultProps"
      node-key="id"
      draggable
      :allow-drop="allowDrop"
      @node-drop="handleNodeDrop"
      @node-click="handleNodeClick"
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
    // 允许拖拽到 flex-container 类型的节点下
    allowDrop(draggingNode, dropNode, type) {
      if (dropNode.data.type === "flex-container") {
        return type !== "prev" && type !== "next";
      } else {
        // 禁止将元素拖拽为根节点
        return dropNode.level !== 1;
      }
    },
    handleNodeDrop(draggingNode, dropNode, dropType, ev) {
      // 拖拽完成后的处理逻辑（通常不需要额外处理）
      this.$emit("node-drop", draggingNode, dropNode, dropType);
    },
    handleNodeClick(data) {
      this.$emit("select-item", data);
    },
  },
};
</script>
