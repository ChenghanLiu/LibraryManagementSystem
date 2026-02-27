<template>
  <div class="login-root">
    <div class="login-card">
      <div class="title">
        <div class="title-main">图书管理系统</div>
        <div class="title-sub">请使用账号密码登录</div>
      </div>

      <el-form label-position="left" label-width="0px">
        <el-form-item>
          <el-input
              placeholder="请输入用户名"
              v-model="rulesForm.username"
          />
        </el-form-item>

        <el-form-item>
          <el-input
              placeholder="请输入密码"
              type="password"
              v-model="rulesForm.password"
          />
        </el-form-item>

        <el-form-item>
          <div class="role-label">角色</div>
          <div class="role-options">
            <el-radio
                v-for="item in backLoginMenus"
                :key="item.roleName"
                v-model="rulesForm.role"
                :label="item.roleName"
            >
              {{ item.roleName }}
            </el-radio>
          </div>
        </el-form-item>

        <el-button type="primary" class="login-btn" @click="login">
          登录
        </el-button>

        <div class="actions">
          <span class="link" @click="register('yonghu')">用户注册</span>
        </div>
      </el-form>
    </div>
  </div>
</template>

<script>
import menu from "@/utils/menu";

export default {
  data() {
    return {
      rulesForm: {
        username: "",
        password: "",
        role: ""
      },
      menus: [],
      tableName: ""
    };
  },

  computed: {
    backLoginMenus() {
      return (this.menus || []).filter(
          item => item.hasBackLogin == "是"
      );
    }
  },

  mounted() {
    this.menus = menu.list();
  },

  methods: {
    register(tableName) {
      this.$storage.set("loginTable", tableName);
      this.$router.push({ path: "/register" });
    },

    login() {
      if (!this.rulesForm.username)
        return this.$message.error("请输入用户名");
      if (!this.rulesForm.password)
        return this.$message.error("请输入密码");
      if (!this.rulesForm.role)
        return this.$message.error("请选择角色");

      for (let i = 0; i < this.menus.length; i++) {
        if (this.menus[i].roleName == this.rulesForm.role) {
          this.tableName = this.menus[i].tableName;
          break;
        }
      }

      this.$http({
        url: `${this.tableName}/login?username=${this.rulesForm.username}&password=${this.rulesForm.password}`,
        method: "post"
      }).then(({ data }) => {
        if (data && data.code === 0) {
          this.$storage.set("Token", data.token);
          this.$storage.set("userId", data.userId);
          this.$storage.set("role", this.rulesForm.role);
          this.$storage.set("sessionTable", this.tableName);
          this.$storage.set("adminName", this.rulesForm.username);
          this.$router.replace({ path: "/index/" });
        } else {
          this.$message.error(data.msg);
        }
      });
    }
  }
};
</script>

<style scoped>
/* 全屏强制覆盖 */
.login-root {
  position: fixed !important;
  inset: 0 !important;
  width: 100vw !important;
  height: 100vh !important;
  z-index: 999999 !important;

  display: flex !important;
  align-items: center !important;
  justify-content: center !important;

  background: #f5f6f8 !important;
  box-sizing: border-box !important;
  padding: 24px !important;
}

/* 登录卡片 */
.login-card {
  width: 380px;
  max-width: 92vw;
  background: #fff;
  border-radius: 14px;
  padding: 24px;
  box-shadow: 0 12px 32px rgba(0, 0, 0, 0.06);
  box-sizing: border-box;
}

.title {
  text-align: center;
  margin-bottom: 20px;
}

.title-main {
  font-size: 20px;
  font-weight: 600;
  color: #111827;
}

.title-sub {
  font-size: 13px;
  color: #6b7280;
  margin-top: 6px;
}

.role-label {
  font-size: 13px;
  color: #6b7280;
  margin-bottom: 6px;
}

.role-options {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
}

.login-btn {
  width: 100%;
  margin-top: 10px;
  height: 40px;
  border-radius: 8px;
}

.actions {
  margin-top: 12px;
  text-align: center;
}

.link {
  font-size: 13px;
  color: #409eff;
  cursor: pointer;
}
</style>