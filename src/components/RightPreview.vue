<template>
  <div>
    <h3>预览区</h3>
    <div class="preview-area">
      <component-preview :tree-data="treeData" />
    </div>
  </div>
</template>

<script>
import ComponentPreview from "./ComponentPreview.vue";

export default {
  props: {
    treeData: {
      type: Array,
      required: true,
    },
  },
  components: {
    ComponentPreview,
  },
};
</script>

<style>
.preview-area {
  border: 1px solid #ebeef5;
  padding: 20px;
  min-height: 400px;
}
</style>
