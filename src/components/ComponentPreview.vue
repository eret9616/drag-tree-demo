<template>
  <div>
    <component
      v-for="item in treeData"
      :is="getComponentName(item.type)"
      :key="item.id"
      v-bind="getComponentProps(item)"
    >
      <!-- 如果有子元素，递归渲染 -->
      <component-preview
        v-if="item.children && item.children.length"
        :tree-data="item.children"
      />
      <!-- 如果是文本类型，显示文本内容 -->
      <span v-else-if="item.type === 'text'">{{
        item.content || "示例文本"
      }}</span>
    </component>
  </div>
</template>

<script>
export default {
  name: "ComponentPreview",
  props: {
    treeData: {
      type: Array,
      required: true,
    },
  },
  methods: {
    getComponentName(type) {
      switch (type) {
        case "text":
          return "div";
        case "image":
          return "img";
        case "square":
        case "circle":
          return "div"; // 方块和圆形都使用 div
        // 根据需要添加更多类型
        default:
          return "div";
      }
    },
    getComponentProps(item) {
      let props = {};
      switch (item.type) {
        case "image":
          props.src = item.src || "https://via.placeholder.com/150";
          props.alt = item.alt || "示例图片";
          props.style = "max-width: 100%;";
          break;
        case "square":
          props.style = `
              width: 100px;
              height: 100px;
              background-color: red;
              margin: 10px;
            `;
          break;
        case "circle":
          props.style = `
              width: 100px;
              height: 100px;
              background-color: blue;
              border-radius: 50%;
              margin: 10px;
            `;
          break;
        // 根据需要添加更多类型
        default:
          break;
      }
      return props;
    },
  },
};
</script>
