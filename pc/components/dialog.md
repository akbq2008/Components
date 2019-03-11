# dialog
<Demo_Dialog/>
案例源码
 ``` vue
 <template>
  <div>
    <el-button @click="open">显示dialog</el-button>
    <Dialog
      :dialogFormVisible="dialogFormVisible"
      :title="dialogTitle"
      @closeDialog="closeDialog"
      @saveDialog="saveDialog"
      >
      <!-- 自定义部分 -->
      <el-form-item label="姓名"><el-input></el-input></el-form-item>
    </Dialog>
  </div>
</template>

<script>
import Dialog from "./Dialog";
import mixins from "tudousi_mixins";
export default {
  mixins: [mixins.dataList],
  components: {
    Dialog
  },
  data() {
    return {};
  },
  methods: {
    open() {
      this.oepnDialog("", "标题");
    },
    saveDialog() {
      this.dialogFormVisible = false;
    }
  }
};
</script>

<style scoped>
</style>

 ```

