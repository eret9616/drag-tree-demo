<template>
  <div>
    <h3>可选元素</h3>
    <el-button
      v-for="item in elements"
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
  data() {
    return {
      elements: [
        { label: "文本", type: "text" },
        { label: "图片", type: "image" },
        { label: "方块", type: "square" }, // 新增方块元素
        { label: "圆形", type: "circle" }, // 新增圆形元素
        // 可以继续添加更多元素
        { label: "Flex 容器", type: "flex-container" },
      ],
    };
  },
  methods: {
    addElement(item) {
      this.$emit("add-element", item);
    },
  },
};
</script>
