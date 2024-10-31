<template>
  <div class="container">
    <!-- 顶部工具栏 -->
    <div class="toolbar">
      <el-button type="primary" @click="saveComponent">保存当前组件</el-button>
    </div>
    <!-- 左侧面板 -->
    <div class="panel left-panel">
      <left-panel
        :custom-components="customComponents"
        @add-element="addElement"
        @add-custom-component="addCustomComponent"
      />
    </div>
    <!-- 中间面板 -->
    <div class="panel center-panel">
      <center-tree
        :tree-data="treeData"
        @select-item="selectItem"
        @node-drop="handleNodeDrop"
      />
    </div>
    <!-- 右侧面板 -->
    <div class="panel right-panel">
      <right-preview :tree-data="treeData" />
      <!-- 属性编辑器 -->
      <property-editor
        :selected-item="selectedItem"
        @update-item="updateItem"
      />
    </div>
  </div>
</template>

<script>
import LeftPanel from "./components/LeftPanel.vue";
import CenterTree from "./components/CenterTree.vue";
import RightPreview from "./components/RightPreview.vue";
import PropertyEditor from "./components/PropertyEditor.vue";

export default {
  name: "App",
  components: {
    LeftPanel,
    CenterTree,
    RightPreview,
    PropertyEditor,
  },
  data() {
    return {
      customComponents: [], // 用于存储用户保存的组件列表
      treeData: [
        {
          id: "root",
          label: "根容器",
          type: "flex-container",
          style: {
            display: "flex",
            "flex-direction": "row",
          },
          children: [],
        },
      ],
      selectedItem: null,
    };
  },
  methods: {
    addCustomComponent(component) {
      debugger;
      // 将自定义组件添加到编辑区
      const componentCopy = JSON.parse(JSON.stringify(component));

      // 需要确保 id 唯一，重新生成 id
      componentCopy.id = Date.now();

      // 添加到 treeData 的子节点中
      this.treeData.children.push(componentCopy);
    },
    saveComponent() {
      this.$prompt("请输入组件名称", "保存组件", {
        confirmButtonText: "确定",
        cancelButtonText: "取消",
      })
        .then(({ value }) => {
          // 用户输入了组件名称
          const componentCopy = JSON.parse(JSON.stringify(this.treeData));
          componentCopy.id = `custom_${Date.now()}`;
          componentCopy.label =
            value || `自定义组件 ${this.customComponents.length + 1}`;
          this.customComponents.push(componentCopy);
          debugger;
          this.$message.success("组件已保存到素材库！");
        })
        .catch(() => {
          // 用户取消了输入
        });
    },
    addElement(item) {
      let _item = item;
      let _label = _item.label;
      if (Array.isArray(item)) {
        _item = item[0];
        _label = "自定义组件" + item.label + ": " + _item.label;
      }
      debugger;
      const newItem = {
        id: Date.now(),
        label: _label,
        type: _item.type,
        style: {},
        children: _item.children ? _item.children : [],
      };

      // 如果需要为新元素添加默认属性，可以在这里设置
      if (_item.type === "text") {
        newItem.content = "示例文本";
      } else if (_item.type === "image") {
        newItem.src = "https://via.placeholder.com/150";
        newItem.alt = "示例图片";
      }
      // 添加到 treeData
      this.treeData[0].children.push(newItem);
    },
    selectItem(item) {
      this.selectedItem = item;
    },
    updateItem(updatedItem) {
      // 这里可以处理更新逻辑，如果需要的话
      this.$set(this.selectedItem, "style", { ...updatedItem.style });
      Object.assign(this.selectedItem, updatedItem);
    },
    handleNodeDrop(draggingNode, dropNode) {
      // 更新拖拽节点的 parentType
      const parentNode = dropNode.parent;
      const parentType = parentNode ? parentNode.data.type : null;
      draggingNode.data.parentType = parentType;
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
