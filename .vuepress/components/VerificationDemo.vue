<template>
  <div class="">
    <el-input v-model="phone" style="width:200px"></el-input>
    <Verification :phone="phone"></Verification>
  </div>
</template>

<script>
import Verification from "./Verification";
export default {
  components: {
    Verification
  },
  data() {
    return {
      phone: ""
    };
  }
};
</script>

<style scoped>
.fx_sp {
  display: flex;
  justify-content: space-between;
}
</style>