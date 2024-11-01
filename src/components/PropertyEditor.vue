<template>
  <div v-if="localSelectedItem">
    <h3>属性编辑</h3>
    <el-button @click="deleteComponent" style="margin: 5px 0" type="">
      删除组件
    </el-button>
    <el-form label-position="top">
      <!-- 子元素的 Flex 属性 -->
      <el-form-item v-if="isChildOfFlexContainer" label="Flex 占比 (flex)">
        <el-input
          :value="localSelectedItem.style.flex"
          @input="(value) => updateStyle('flex', value)"
          placeholder="如：1"
        ></el-input>
      </el-form-item>
      <el-form-item v-if="isChildOfFlexContainer" label="对齐方式 (align-self)">
        <el-select
          v-model="localSelectedItem.style['align-self']"
          @change="onStyleChange"
        >
          <el-option label="自动" value="auto"></el-option>
          <el-option label="起始" value="flex-start"></el-option>
          <el-option label="居中" value="center"></el-option>
          <el-option label="结束" value="flex-end"></el-option>
          <el-option label="拉伸" value="stretch"></el-option>
        </el-select>
      </el-form-item>
      <!-- Flex 属性设置 -->
      <el-form-item v-if="isFlexContainer" label="Flex换行">
        <el-select
          v-model="localSelectedItem.style['flex-wrap']"
          @change="onStyleChange"
        >
          <el-option label="换行" value="wrap"></el-option>
          <el-option label="不换行" value="nowrap"></el-option>
        </el-select>
      </el-form-item>
      <!-- Flex 属性设置 -->
      <el-form-item v-if="isFlexContainer" label="Flex 方向">
        <el-select
          v-model="localSelectedItem.style['flex-direction']"
          @change="onStyleChange"
        >
          <el-option label="横向" value="row"></el-option>
          <el-option label="纵向" value="column"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item v-if="isFlexContainer" label="主轴对齐方式">
        <el-select
          v-model="localSelectedItem.style['justify-content']"
          @change="onStyleChange"
        >
          <el-option label="起始" value="flex-start"></el-option>
          <el-option label="居中" value="center"></el-option>
          <el-option label="结束" value="flex-end"></el-option>
          <el-option label="两端对齐" value="space-between"></el-option>
          <el-option label="等间距" value="space-around"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item v-if="isFlexContainer" label="交叉轴对齐方式">
        <el-select
          v-model="localSelectedItem.style['align-items']"
          @change="onStyleChange"
        >
          <el-option label="起始" value="flex-start"></el-option>
          <el-option label="居中" value="center"></el-option>
          <el-option label="结束" value="flex-end"></el-option>
          <el-option label="拉伸" value="stretch"></el-option>
        </el-select>
      </el-form-item>
      <!-- 尺寸设置 -->
      <el-form-item label="宽度">
        <el-input
          :value="localSelectedItem.style.width"
          @input="(value) => updateStyle('width', value)"
          placeholder="如：100px 或 50%"
        ></el-input>
      </el-form-item>
      <el-form-item label="高度">
        <el-input
          :value="localSelectedItem.style.height"
          @input="(value) => updateStyle('height', value)"
          placeholder="如：100px 或 50%"
        ></el-input>
      </el-form-item>
      <!-- Margin 设置 -->
      <el-form-item label="Margin">
        <el-input
          :value="localSelectedItem.style.margin"
          @input="(value) => updateStyle('margin', value)"
          placeholder="如：10px"
        ></el-input>
      </el-form-item>
      <!-- Padding 设置 -->
      <el-form-item label="Padding">
        <el-input
          :value="localSelectedItem.style.padding"
          @input="(value) => updateStyle('padding', value)"
          placeholder="如：10px"
        ></el-input>
      </el-form-item>
      <!-- 其他属性可以根据需要添加 -->
    </el-form>
  </div>
</template>

<script>
export default {
  props: {
    selectedItem: {
      type: Object,
      default: null,
    },
    treeData: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      localSelectedItem: null,
    };
  },
  watch: {
    selectedItem: {
      handler(newVal) {
        // 创建 selectedItem 的深拷贝
        this.localSelectedItem = JSON.parse(JSON.stringify(newVal));
        // test
        window.localSelectedItem = this.localSelectedItem;
      },
      immediate: true,
    },
  },
  computed: {
    isFlexContainer() {
      return (
        this.localSelectedItem &&
        this.localSelectedItem.type === "flex-container"
      );
    },
    isChildOfFlexContainer() {
      return (
        this.localSelectedItem &&
        this.localSelectedItem.parentType === "flex-container"
      );
    },
  },
  methods: {
    deleteComponent() {
      window.alert("删除组件");
      const deleteNodeById = (data, id) => {
        for (let i = 0; i < data.length; i++) {
          const node = data[i];
          if (node.id === id) {
            data.splice(i, 1); // 找到节点，直接删除
            return true;
          }
          if (node.children && node.children.length > 0) {
            const found = deleteNodeById(node.children, id); // 递归查找
            if (found) return true;
          }
        }
        return false;
      };
      deleteNodeById(this.treeData, this.localSelectedItem.id);
    },
    updateStyle(prop, value) {
      this.$set(this.localSelectedItem.style, prop, value);
      this.$emit("update-item", this.localSelectedItem);
    },
    onStyleChange() {
      this.$emit("update-item", this.localSelectedItem);
    },
  },
};
</script>
