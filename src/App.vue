<template>
  <div class="container">
    <!-- 左侧面板 -->
    <div class="panel left-panel">
      <left-panel @add-element="addElement" />
    </div>
    <!-- 中间面板 -->
    <div class="panel center-panel">
      <center-tree :tree-data="treeData" />
    </div>
    <!-- 右侧面板 -->
    <div class="panel right-panel">
      <right-preview :tree-data="treeData" />
    </div>
  </div>
</template>

<script>
import LeftPanel from "./components/LeftPanel.vue";
import CenterTree from "./components/CenterTree.vue";
import RightPreview from "./components/RightPreview.vue";

export default {
  name: "App",
  components: {
    LeftPanel,
    CenterTree,
    RightPreview,
  },
  data() {
    return {
      treeData: [],
    };
  },
  methods: {
    addElement(item) {
      const newItem = {
        id: Date.now(),
        label: item.label,
        type: item.type,
        children: [],
      };
      // 如果需要为新元素添加默认属性，可以在这里设置
      if (item.type === "text") {
        newItem.content = "示例文本";
      } else if (item.type === "image") {
        newItem.src = "https://via.placeholder.com/150";
        newItem.alt = "示例图片";
      }
      // 添加到 treeData
      this.treeData.push(newItem);
    },
  },
};
</script>

<style>
.container {
  display: flex;
  height: 100vh;
}
.panel {
  flex: 1;
  overflow: auto;
}
.left-panel {
  border-right: 1px solid #ebeef5;
  padding: 20px;
}
.center-panel {
  border-right: 1px solid #ebeef5;
  padding: 20px;
}
.right-panel {
  padding: 20px;
}
</style>
