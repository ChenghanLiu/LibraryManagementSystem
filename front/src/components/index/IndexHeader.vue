
<template>
    <!-- <el-header>
        <el-menu background-color="#00c292" text-color="#FFFFFF" active-text-color="#FFFFFF" mode="horizontal">
            <div class="fl title">{{this.$project.projectName}}</div>
            <div class="fr logout" style="display:flex;">
                <el-menu-item index="3">
                    <div>{{this.$storage.get('role')}} {{this.$storage.get('adminName')}}</div>
                </el-menu-item>
                <el-menu-item @click="onLogout" index="2">
                    <div>退出登录</div>
                </el-menu-item>
            </div>
        </el-menu>
    </el-header> -->
    <div class="navbar" :style="{backgroundColor:heads.headBgColor,height:heads.headHeight,boxShadow:heads.headBoxShadow,lineHeight:heads.headHeight}">
        <div class="title-menu" :style="{justifyContent:heads.headTitleStyle=='1'?'flex-start':'center'}">
            <el-image v-if="heads.headTitleImg" class="title-img" :style="{width:heads.headTitleImgWidth,height:heads.headTitleImgHeight,boxShadow:heads.headTitleImgBoxShadow,borderRadius:heads.headTitleImgBorderRadius}" :src="heads.headTitleImgUrl" fit="cover"></el-image>
            <div class="title-name" :style="{color:heads.headFontColor,fontSize:heads.headFontSize}">{{this.$project.projectName}}</div>
            <!--             <img src="../../../../img/logo.jpg" style="width: 60px;height: 60px;border-radius:60px"> -->
        </div>
        <div class="right-menu">
            <div class="user-info" :style="{color:heads.headUserInfoFontColor,fontSize:heads.headUserInfoFontSize}">{{this.$storage.get('role')}} {{this.$storage.get('adminName')}}</div>

            <div class="logout" :style="{color:heads.headLogoutFontColor,fontSize:heads.headLogoutFontSize}" @click="onLogout">退出登录</div>
        </div>
    </div>
</template>

<script>
    export default {
        data() {
            return {
                dialogVisible: false,
                ruleForm: {},
                user: {},
                heads: {"headLogoutFontHoverColor":"#fff","headFontSize":"25px","headUserInfoFontColor":"rgba(231, 231, 231, 1)","headBoxShadow":"0px 0px 0px 10px #FFFF66","headTitleImgHeight":"44px","headLogoutFontHoverBgColor":"rgba(170, 153, 153, 0.41)","headFontColor":"rgba(255, 255, 255, 1)","headTitleImg":false,"headHeight":"70px","headTitleImgBorderRadius":"22px","headTitleImgUrl":"http://codegen.caihongy.cn/20201021/cc7d45d9c8164b58b18351764eba9be1.jpg","headBgColor":"#19A97B","headTitleImgBoxShadow":"0 1px 6px #444","headLogoutFontColor":"rgba(231, 231, 231, 1)","headUserInfoFontSize":"16px","headTitleImgWidth":"44px","headTitleStyle":"2","headLogoutFontSize":"16px"},
            };
        },
        created() {
            this.setHeaderStyle()
        },
        mounted() {
            let sessionTable = this.$storage.get("sessionTable")
            this.$http({
                url: sessionTable + '/session',
                method: "get"
            }).then(({
                         data
                     }) => {
                if (data && data.code === 0) {
                    this.user = data.data;
                } else {
                    let message = this.$message
                    message.error(data.msg);
                }
            });
        },
        methods: {
            onLogout() {
                let storage = this.$storage
                let router = this.$router
                storage.clear()
                router.replace({
                    name: "login"
                });
            },
          onIndexTap() {
            // 跳到独立前台入口（同域同端口，不会发生 8084->8080 的混乱）
            window.location.href = "/tushuguanli/front/index.html";
          },
            setHeaderStyle() {
                this.$nextTick(()=>{
                    document.querySelectorAll('.navbar .right-menu .logout').forEach(el=>{
                        el.addEventListener("mouseenter", e => {
                            e.stopPropagation()
                            el.style.backgroundColor = this.heads.headLogoutFontHoverBgColor
                            el.style.color = this.heads.headLogoutFontHoverColor
                        })
                        el.addEventListener("mouseleave", e => {
                            e.stopPropagation()
                            el.style.backgroundColor = "transparent"
                            el.style.color = this.heads.headLogoutFontColor
                        })
                    })
                })
            },
        }
    };
</script>


<style lang="scss" scoped>
.navbar {
  width: 100%;
  padding: 0 24px;
  box-sizing: border-box;
  position: relative;
  z-index: 111;

  /* 灰蓝渐变顶栏 */
  background: linear-gradient(90deg, #eef2f7 0%, #e6ecf5 100%) !important;
  border-bottom: 1px solid #d8e1ee;
  box-shadow: 0 2px 10px rgba(40, 60, 90, 0.05) !important;

  display: flex;
  align-items: center;
}

/* 左侧标题区域 */
.title-menu {
  display: flex;
  align-items: center;
  gap: 12px;
  height: 100%;
  width: 100%;
}

.title-img {
  width: 32px !important;
  height: 32px !important;
  border-radius: 8px !important;
  box-shadow: none !important;
}

/* 系统名称 */
.title-name {
  font-size: 16px !important;
  font-weight: 600 !important;
  color: #2c3e50 !important;
  letter-spacing: 0.3px;
}

/* 右侧区域 */
.right-menu {
  position: absolute;
  right: 24px;
  top: 0;
  height: 100%;

  display: flex;
  align-items: center;
  justify-content: flex-end;
  gap: 8px;
}

/* 用户信息 */
.user-info {
  font-size: 13px !important;
  color: #4a6078 !important;
  padding: 0 10px !important;
  border-right: 1px solid #d8e1ee;
  line-height: 1;
}

/* 按钮 */
.logout {
  font-size: 13px !important;
  color: #3a5a8f !important;
  padding: 8px 12px !important;
  line-height: 1;
  cursor: pointer;
  border-radius: 6px;
  transition: all 0.15s ease;
}

/* hover */
.logout:hover {
  background: #dbe6f5;
  color: #1f3f75;
}
</style>