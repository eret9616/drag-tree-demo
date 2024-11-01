<template>
  <div class="preview-area">
    <component-preview
      v-for="item in treeData"
      :key="item.id"
      :tree-data="item"
    />
  </div>
</template>

<script>
import ComponentPreview from "./ComponentPreview.vue";

export default {
  name: "RightPreview",
  components: {
    ComponentPreview,
  },
  props: {
    treeData: {
      type: Array,
      required: true,
    },
  },
};
</script>

<style>
.preview-area {
  border: 1px dotted black;
  width: 392px;
  height: 871.11px;
}
</style>
