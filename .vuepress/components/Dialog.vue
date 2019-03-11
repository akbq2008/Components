<template>
  <div>
    <el-dialog :title="title" :visible="dialogFormVisible" @close="close">
      <el-form :label-width="width">
        <slot></slot>
        <el-form-item class="cr">
          <el-button @click="close"> 取消</el-button>
          <el-button @click="save">确定</el-button>
        </el-form-item>
      </el-form>
    </el-dialog>
  </div>
</template>

<script>
export default {
  props: {
    width: {
      type: String,
      default: "80px"
    },
    title: {
      type: String,
      default: "标题"
    },
    dialogFormVisible: {
      type: Boolean,
      default: false
    }
  },
  methods: {
    save() {
      this.$emit("saveDialog");
    },
    close() {
      this.$emit("closeDialog", false);
    }
  }
};
</script>

<style scoped>
</style>