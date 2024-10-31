<template>
  <div>
    <h3>素材库</h3>
    <!-- 内置组件列表 -->
    <el-button
      v-for="item in builtInComponents"
      :key="item.type"
      @click="addElement(item)"
      style="margin: 5px 0"
      type="primary"
    >
      {{ item.label }}
    </el-button>
    <!-- 自定义组件列表 -->
    <h3 style="margin-top: 20px">自定义组件</h3>
    <el-button
      v-for="item in customComponents"
      :key="item.type"
      @click="addElement(item)"
      style="margin: 5px 0"
      type="primary"
    >
      {{ item.label }}
    </el-button>
  </div>
</template>

<script>
export default {
  props: {
    customComponents: {
      type: Array,
      default: () => [],
    },
  },
  data() {
    return {
      builtInComponents: [
        // 内置组件列表
        { id: "text", label: "文本", type: "text" },
        { id: "image", label: "图片", type: "image" },
        { id: "square", label: "方块", type: "square" },
        { id: "circle", label: "圆形", type: "circle" },
        { id: "flex-container", label: "Flex 容器", type: "flex-container" },
      ],
      defaultProps: {
        children: "children",
        label: "label",
      },
    };
  },
  methods: {
    addElement(item) {
      this.$emit("add-element", item);
    },
  },
};
</script>
