# Vue mixins

---
介绍
---
通过mixins来抽离中后台(基于element-admin的中后台管理系统)逻辑代码，将常用的后台逻辑进行封装，目前抽离出了2套逻辑代码，一个主要用于列表，一个主要是用于添加和修改的逻辑。


##  列表
``` js
let dataList = {
  data() {
    return {
      basic: {},
      list: [], //列表数据,
      isEdit: false, //是否编辑状态
      currentId: "", //当前编辑项目
      dialogFormVisible: false, //dialog toggle
      dialogTitle: "", //dialog标题
    }
  },
  created() {

  },
  methods: {
    /**
     * 
     * @param {*} type   dialog类型，添加或者编辑
     * @param {*} title  dialog标题
     */
    oepnDialog(type, title) {
      this.dialogFormVisible = true;
      this.dialogTitle = title;
    },
    /**
     * 关闭dialog
     */
    closeDialog() {
      this.dialogFormVisible = false;
    },
    /**
     * 
     * @param {*} formData  参数
     * @param {*} fun 函数名
     */
    async saveDialogFun(formData, fun) {
      let res = await fun(formData);
      this.sortFun(res);
      this.closeDialog();
    },
    /**
     * 
     * @param {*} formData 参数
     * @param {*} fun 函数名字
     * @param {*} fn 回调函数
     */
    async getData(formData, fun, fn) {
      try {
        let res = await fun(formData);
        if (res.data.code === 200) {
          if (fn) {
            fn(res);
          } else {
            const data = res.data.data;
            this.list = data.item;
          }
        }
      } catch (error) {
        console.log(error);
        this.$message.error(error);
      }
    },
    /**
     * 
     * @param {*} title confirm弹窗标题
     * @param {*} form   函数参数 
     * @param {*} fun  执行操作的函数
     * @param {*} fn  回调函数
     */
    change(title, form, fun, fn) {
      let _this = this;
      this.$confirm(title ? title : '是否要更改状态？', "提示", {
          confirmButtonText: "确定",
          cancelButtonText: "取消",
          type: "warning"
        })
        .then(() => {
          try {
            if (fn) {
              fn();
            } else {
              fun(form).then(res => {
                _this.sortFun(res);
              });
            }
          } catch (error) {
            console.log(error);
            this.$message.error(error);
          }
        })
        .catch(err => {
          this.$message({
            type: "info",
            message: err !== "cancel" ? err : "已取消删除"
          });
        });
    },
    /**
     * 
     * @param {*} key   form键名
     * @param {*} id    id值
     * @param {*} fun   回调函数
     */
    delete(key, id, fun, title) {
      this.$confirm(title ? title : '确定要删除吗？', "提示", {
          confirmButtonText: "确定",
          cancelButtonText: "取消",
          type: "warning"
        })
        .then(() => {
          try {
            let form = {
              [key]: id
            };
            fun(form).then(res => {
              if (res.data.code === 200) {
                this.$message({
                  message: "操作成功",
                  type: "success"
                });
                this.getListData();
              } else {
                this.$message.error(res.data.message);
              }
            });
          } catch (error) {
            this.$message.error(error);
          }
        })
        .catch(err => {
          this.$message({
            type: "info",
            message: err !== "cancel" ? err : "已取消删除"
          });
        });
    },
    /**
     * 
     * @param {*} url 路径
     * @param {*} data  数据
     */
    turnTo(url, data) {
      if (data) {
        this.$router.push(`${url}?data=${JSON.stringify(data)}`);
      } else {
        this.$router.push(url);
      }
    },
    /**
     * 
     * @param {*} key 键
     * @param {*} id id值
     * @param {*} fun  回调函数
     */
    async sort(key, id, fun) {
      let form = {
        [key]: +id
      };
      try {
        let res = await fun(form);
        this.sortFun(res);
      } catch (error) {
        this.$message.error(error);
      }
    },
    /**
     * 
     * @param {*} res 接收到的参数
     */
    sortFun(res) {
      if (res.data.code === 200) {
        this.$message({
          message: "操作成功",
          type: "success"
        });
        this.getListData();
      } else {
        this.$message.error(res.data.message);
      }
    }
  },
}
export default dataList;
```

##  添加和编辑

``` js
let editData = {
  data() {
    return {
      basic: {
        url: "", //图片url
        img_id: "", //图片id
      }, //编辑数据
    }
  },
  created() {

  },
  methods: {
    /**
     * 
     * @param {*} addFormData 函数参数
     * @param {*} addFun   添加函数
     * @param {*} fn    回调函数
     */
    async addFun(addFormData, addFun, fn) {
      try {
        let res = "";
        if (fn) {
          fn();
        } else {
          res = await addFun(addFormData);
        }
        this.operation(res);
      } catch (error) {
        console.log(error);
        this.$message.error(error);
      }
    },
    /**
     * 
     * @param {*} queryKey  编辑路由
     * @param {*} editFormData 编辑函数参数
     * @param {*} editFun   编辑函数
     * @param {*} fn 回调函数
     */
    async editFun(queryKey, editFormData, editFun, fn) {
      try {
        let res = "";
        if (this.$route.query[queryKey]) {
          if (fn) {
            fn();
          } else {
            res = await editFun(editFormData);
          }
        }
        this.operation(res);
      } catch (error) {
        console.log(error);
        this.$message.error(error);
      }
    },
    /**
     * 
     * @param {*} res  接收到的参数
     */
    operation(res) {
      if (res.data.code === 200) {
        this.$message({
          message: "操作成功",
          type: "success"
        });
        this.$router.go(-1);
      } else {
        this.$message.error(res.data.message);
      }
    },
    /**
     * 
     * @param {*} key 如果传了参数。就让指定参数的图片为空，否则就使用默认的
     */
    emptyImgUrl(key) {
      if (key) {
        this.basic[key] = "";
        this.$forceUpdate();
      } else {
        this.basic.url = "";
      }
    },
  },
}
export default editData;
```