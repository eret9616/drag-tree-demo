<template>
  <component
    :is="getComponentName(treeData.type)"
    :key="treeData.id"
    :style="getComputedStyle(treeData)"
    v-bind="getComponentProps(treeData)"
  >
    <!-- 如果有子元素，递归渲染 -->
    <template v-if="treeData.children && treeData.children.length">
      <component-preview
        v-for="child in treeData.children"
        :key="child.id"
        :tree-data="child"
      />
    </template>
    <!-- 如果是文本类型，显示文本内容 -->
    <span v-else-if="treeData.type === 'text'">
      {{ treeData.content || "示例文本" }}
    </span>
  </component>
</template>

<script>
export default {
  name: "ComponentPreview",
  props: {
    treeData: {
      type: Object,
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
        case "flex-container":
          return "div";
        default:
          return "div";
      }
    },
    getComponentProps(item) {
      let props = {};
      if (item.type === "image") {
        props.src = item.src || "https://via.placeholder.com/150";
        props.alt = item.alt || "示例图片";
      }
      return props;
    },
    getComputedStyle(item) {
      let style = { ...item.style };

      switch (item.type) {
        case "square":
          style.width = style.width || "100px";
          style.height = style.height || "100px";
          style["background-color"] = style["background-color"] || "red";
          style.margin = style.margin || "10px";
          break;
        case "circle":
          style.width = style.width || "100px";
          style.height = style.height || "100px";
          style["background-color"] = style["background-color"] || "blue";
          style["border-radius"] = "50%";
          style.margin = style.margin || "10px";
          break;
        case "flex-container":
          style.display = "flex";
          style["flex-direction"] = style["flex-direction"] || "row";
          style["justify-content"] = style["justify-content"] || "flex-start";
          style["align-items"] = style["align-items"] || "flex-start";
          break;
        default:
          break;
      }

      return style;
    },
  },
};
</script>
