<template>
  <el-aside class="index-aside" width="200px">
    <div class="index-aside-inner">
      <el-menu default-active="1">
        <el-menu-item @click="menuHandler('/')" index="1">
          <!-- <i class="el-icon-s-home"></i> -->
          首页
        </el-menu-item>
        <sub-menu
          v-for="menu in menuList"
          :key="menu.menuId"
          :menu="menu"
          :dynamicMenuRoutes="dynamicMenuRoutes"
        ></sub-menu>
      </el-menu>
    </div>
  </el-aside>
</template>
<script>
import SubMenu from "@/components/index/IndexAsideSub";
export default {
  data() {
    return {
      menuList: [],
      dynamicMenuRoutes: []
    };
  },
  components: {
    SubMenu
  },
  mounted() {
    // 获取动态菜单数据并且渲染
    this.menuList = JSON.parse(sessionStorage.getItem("menuList") || "[]");
    this.dynamicMenuRoutes = JSON.parse(
      sessionStorage.getItem("dynamicMenuRoutes") || "[]"
    );
  },
  methods: {
    menuHandler(path) {
      this.$router.push({ path: path });
    }
  }
};
</script>
<style lang="scss" scoped>
.index-aside {
  margin-top: 70px;
  width: 200px;
  background: #f2f6fc;
  border-right: 1px solid #d8e1ee;

  .index-aside-inner {
    width: 100%;
    height: 100%;
    overflow-y: auto;
  }
}

/* hover 效果 */
::v-deep .el-menu-item:hover {
  background-color: #e4edf8 !important;
}

/* 激活项 */
::v-deep .el-menu-item.is-active {
  background-color: #d6e4f7 !important;
  font-weight: 600;
}

/* 子菜单标题 */
::v-deep .el-submenu__title:hover {
  background-color: #e4edf8 !important;
}
</style>

