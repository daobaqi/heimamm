<template>
  <!-- 新增对话框 -->
  <el-dialog class="edit-dialog" center title="修改学科" :visible.sync="$parent.editFormVisible">
    <el-form ref="editForm" :model="editForm" :rules="editFormRules">
      <el-form-item label="学科编号" prop="rid" :label-width="formLabelWidth">
        <el-input v-model="editForm.rid" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="学科名称" prop="name" :label-width="formLabelWidth">
        <el-input v-model="editForm.name" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="学科简称" :label-width="formLabelWidth">
        <el-input v-model="editForm.short_name" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="学科简介" :label-width="formLabelWidth">
        <el-input v-model="editForm.intro" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="学科备注" :label-width="formLabelWidth">
        <el-input v-model="editForm.remark" autocomplete="off"></el-input>
      </el-form-item>
    </el-form>
    <div slot="footer" class="dialog-footer">
      <el-button @click="$parent.editFormVisible = false">取 消</el-button>
      <el-button type="primary" @click="submitForm">确 定</el-button>
    </div>
  </el-dialog>
</template>

<script>
// 导入 编辑接口
import { subjectEdit } from "../../../../api/subject";
export default {
  data() {
    return {
      // 表单数据
      editForm: {
        // 编号
        rid: "",
        // 名字
        name: "",
        // 简称
        short_name: "",
        // 简介
        intro: "",
        // 备注
        remark: ""
      },
      // 宽度
      formLabelWidth: "80px",
      // 添加表单验证规则
      editFormRules: {
        rid: [{ required: true, message: "学科编号不能为空", trigger: "blur" }],
        name: [{ required: true, message: "学科名称不能为空", trigger: "blur" }]
      }
    };
  },
  methods: {
    submitForm() {
      this.$refs.editForm.validate(valid => {
        if (valid) {
          // 对
          subjectEdit(this.editForm).then(res => {
            // window.console.log(res);
            if (res.code === 201) {
              this.$message.warning("学科编号已经存在了，请重新输入");
            } else if (res.code === 200) {
              this.$message.success("恭喜你，修改成功啦！");
              // 关闭弹框
              this.$parent.editFormVisible = false;
              // 调用父组件的 获取数据方法
              this.$parent.getData();
            }
          });
        } else {
          // 错
          this.$message.error("小老弟，数据不太对哦，检查一下呗！");
          return false;
        }
      });
    }
  }
};
</script>

<style lang="less">
.edit-dialog {
  // 设置宽度
  .el-dialog {
    width: 602px;
  }
  // 对话框
  .el-dialog__header {
    background: linear-gradient(to right, #01c4fa, #1394fa);
    // 文字
    span {
      color: white;
    }
    i {
      color: white;
    }
  }
}
</style>
