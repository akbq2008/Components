<template>
  <div style="margin-top:30px;">
    <el-button type="primary" @click="dialogVisible = true">登录</el-button>
    <Login :dialogVisible="dialogVisible" @close="closeBox"></Login>
    <!-- v-if="!userName" -->
  </div>
</template>

<script>
import Login from "./Login";
export default {
  components: {
    Login
  },
  data() {
    return {
      dialogVisible: false
    };
  },
  methods: {
    closeBox(bol) {
      this.dialogVisible = bol;
    }
  }
};
</script>

<style scoped>
</style>