<!-- eslint-disable -->
<template>
  <div>
    <h3>元素结构</h3>
    <el-tree
      :icon-class="'rmDefaultIcon'"
      :data="treeData"
      :props="defaultProps"
      :show-expand-icon="false"
      node-key="id"
      draggable
      :allow-drop="allowDrop"
      @node-drop="handleNodeDrop"
      @node-click="handleNodeClick"
      default-expand-all
      :expand-on-click-node="false"
      highlight-current
      style="border: 1px solid #ebeef5; padding: 10px"
    >
      <div slot-scope="{ node, data }" class="slot-scope">
        <span v-if="data.type === 'flex-container'">+</span> {{ data.label }}
      </div>
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
        // return type !== "prev" && type !== "next";
        return true;
      } else {
        // 禁止将元素拖拽为根节点
        return dropNode.level !== 1;
        // return false;
      }
    },
    handleNodeDrop(draggingNode, dropNode, dropType, ev) {
      debugger;
      // 如果是不是flex-container，不能添加进去
      if (dropNode.data.type !== "flex-container") {
        return;
      }
      // 拖拽完成后的处理逻辑（通常不需要额外处理）
      this.$emit("node-drop", draggingNode, dropNode, dropType);
    },
    handleNodeClick(data) {
      this.$emit("select-item", data);
    },
  },
};
</script>
<style lang="less"></style>
