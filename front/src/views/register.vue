<template>
  <div class="register-root">
    <div class="register-card">
      <div class="title">
        <div class="title-main">图书管理系统注册</div>
        <div class="title-sub">创建账号后可登录并完善个人信息</div>
      </div>

      <el-form class="rgs-form" label-position="top">
        <el-form-item label="账号">
          <el-input v-model="ruleForm.username" autocomplete="off" placeholder="请输入账号" />
        </el-form-item>

        <el-form-item label="密码">
          <el-input
              type="password"
              v-model="ruleForm.password"
              autocomplete="off"
              show-password
              placeholder="请输入密码"
          />
        </el-form-item>

        <el-form-item label="重复密码">
          <el-input
              type="password"
              v-model="ruleForm.repetitionPassword"
              autocomplete="off"
              show-password
              placeholder="请再次输入密码"
          />
        </el-form-item>

        <el-form-item v-if="tableName === 'yonghu'" label="用户姓名">
          <el-input v-model="ruleForm.yonghuName" autocomplete="off" placeholder="请输入姓名" />
        </el-form-item>

        <el-form-item v-if="tableName === 'yonghu'" label="联系方式">
          <el-input v-model="ruleForm.yonghuPhone" autocomplete="off" placeholder="请输入手机号" />
        </el-form-item>

        <el-form-item v-if="tableName === 'yonghu'" label="用户身份证号">
          <el-input v-model="ruleForm.yonghuIdNumber" autocomplete="off" placeholder="请输入身份证号" />
        </el-form-item>

        <el-form-item v-if="tableName === 'yonghu'" label="邮箱">
          <el-input v-model="ruleForm.yonghuEmail" autocomplete="off" placeholder="请输入邮箱" />
        </el-form-item>

        <div class="btn-row">
          <el-button class="btn" type="primary" @click="login()">注册</el-button>
          <el-button class="btn" @click="close()">取消</el-button>
        </div>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      ruleForm: {},
      tableName: "",
      rules: {},
      sexTypesOptions: []
    };
  },
  mounted() {
    let table = this.$storage.get("loginTable");
    this.tableName = table;

    // 级联表的下拉框查询（你原来留空的我也不动）
  },
  methods: {
    // 获取uuid（保留你原来的）
    getUUID() {
      return new Date().getTime();
    },
    close() {
      this.$router.push({ path: "/login" });
    },
    // 注册（保留你原来的方法名 login，不动外部调用）
    login() {
      if (!this.ruleForm.username) {
        this.$message.error("账号不能为空");
        return;
      }
      if (!this.ruleForm.password) {
        this.$message.error("密码不能为空");
        return;
      }
      if (!this.ruleForm.repetitionPassword) {
        this.$message.error("重复密码不能为空");
        return;
      }
      if (this.ruleForm.repetitionPassword != this.ruleForm.password) {
        this.$message.error("密码和重复密码不一致");
        return;
      }
      if (!this.ruleForm.yonghuName && "yonghu" == this.tableName) {
        this.$message.error("用户姓名不能为空");
        return;
      }
      if (
          "yonghu" == this.tableName &&
          this.ruleForm.yonghuPhone &&
          !this.$validate.isMobile(this.ruleForm.yonghuPhone)
      ) {
        this.$message.error("手机应输入手机格式");
        return;
      }
      if (!this.ruleForm.yonghuIdNumber && "yonghu" == this.tableName) {
        this.$message.error("用户身份证号不能为空");
        return;
      }
      if (
          "yonghu" == this.tableName &&
          this.ruleForm.yonghuEmail &&
          !this.$validate.isEmail(this.ruleForm.yonghuEmail)
      ) {
        this.$message.error("邮箱应输入邮件格式");
        return;
      }

      this.$http({
        url: `${this.tableName}/register`,
        method: "post",
        data: this.ruleForm
      }).then(({ data }) => {
        if (data && data.code === 0) {
          this.$message({
            message: "注册成功,请登录后在个人中心页面补充个人数据",
            type: "success",
            duration: 1500,
            onClose: () => {
              this.$router.replace({ path: "/login" });
            }
          });
        } else {
          this.$message.error(data.msg);
        }
      });
    }
  }
};
</script>

<style lang="scss" scoped>
/* 全屏覆盖 + 居中，跟 login 同一套 */
.register-root {
  position: fixed !important;
  inset: 0 !important;
  width: 100vw !important;
  height: 100vh !important;
  z-index: 999999 !important;

  display: flex !important;
  align-items: center !important;
  justify-content: center !important;

  background: #f5f6f8 !important;
  padding: 24px !important;
  box-sizing: border-box !important;
}

.register-card {
  width: 420px;
  max-width: 92vw;
  background: #fff;
  border: 1px solid #ebeef5;
  border-radius: 14px;
  padding: 24px;
  box-shadow: 0 12px 32px rgba(0, 0, 0, 0.06);
  box-sizing: border-box;
}

.title {
  text-align: center;
  margin-bottom: 18px;
}
.title-main {
  font-size: 20px;
  font-weight: 600;
  color: #111827;
}
.title-sub {
  margin-top: 6px;
  font-size: 13px;
  color: #6b7280;
}

.rgs-form {
  ::v-deep .el-form-item__label {
    padding-bottom: 6px;
    line-height: 18px;
    color: #6b7280;
    font-size: 13px;
  }
  ::v-deep .el-input__inner {
    height: 42px;
    line-height: 42px;
    border-radius: 10px;
  }
}

.btn-row {
  display: flex;
  gap: 12px;
  margin-top: 8px;
}
.btn {
  flex: 1;
  height: 42px;
  border-radius: 10px;
}
</style>
