<template>
  <div class="bg">
    <div class="login-wrap animated fadeIn">
      <h3>Auto Vue</h3>
      <h3>{{$t('m.login.introduction')}}</h3>
      <el-form ref="form" :model="form" :rules="rules" label-width="0px">
        <el-form-item prop="name">
          <el-input :placeholder="$t('m.login.name_holder')" v-model="form.name"></el-input>
        </el-form-item>
        <el-form-item prop="password">
          <el-input :placeholder="$t('m.login.password_holder')" v-model="form.password" type="password"></el-input>
        </el-form-item>
        <el-form-item>
          <el-row type="flex" justify="space-between">
            <el-checkbox v-model="isMemery" style="color:#eee">{{$t('m.login.remember')}}</el-checkbox>
            <a href @click.prevent="openMsg" style="color:#eee">{{$t('m.login.forget')}}</a>
          </el-row>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="Login('form')">{{$t('m.login.button')}}</el-button>
        </el-form-item>
      </el-form>
    </div>

    <!-- 粒子漂浮物 -->
    <vue-particles color="#fff" :particleOpacity="0.7" :particlesNumber="30" shapeType="star" :particleSize="5" linesColor="#fff" :linesWidth="2" :lineLinked="true" :lineOpacity="0.4" :linesDistance="150" :moveSpeed="3" :hoverEffect="true" hoverMode="grab" :clickEffect="true" clickMode="push"></vue-particles>
  </div>
</template>
<script>
// 引入粒子特效插件并注册
import Vue from "vue";
import router from "../router/index";
import generateRoutes from "../router/parse";
import VueParticles from "vue-particles";
Vue.use(VueParticles);
export default {
  name: "signin",
  data() {
    // var checkone = (rule, value, callback) => {
    //   let regphone = /(^0{0,1}1[3|4|5|6|7|8|9][0-9]{9}$)/;
    //   let regmail = /^[_\.0-9a-z-]+@([0-9a-z][0-9a-z-]+\.){1,4}[a-z]{2,3}$/;
    //   if (value === "") {
    //     callback(new Error("请输入手机号/邮箱"));
    //   } else {
    //     if (!isNaN(value)) {dfa
    //       if (!regphone.test(value)) {
    //         callback(new Error("请输入正确手机号"));
    //         return false;
    //       }
    //       callback();
    //     }
    //     if (value.indexOf("@") != -1) {
    //       if (!regmail.test(value)) {
    //         callback(new Error("请输入正确邮箱"));
    //         return false;
    //       }
    //       callback();
    //     }
    //     callback(new Error("请输入正确手机/邮箱"));
    //   }
    // };
    return {
      form: {
        name: localStorage.userInfo || "admin",
        password: localStorage.passwordInfo || "654321"
      },
      isMemery: false,
      rules: {
        name: [
          {
            required: true,
            message: this.$t("m.login.name_tip"),
            trigger: "blur"
            // validator: checkone
          }
        ],
        password: [
          {
            required: true,
            message: this.$t("m.login.password_tip"),
            trigger: "blur"
          }
        ]
      }
    };
  },
  methods: {
    Login(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          this.$http
            .post("login", {
              username: this.form.name,
              password: this.form.password
            })
            .then(res => {
              localStorage.userName = res.data.userName;
              localStorage.userId = res.data.userId;
              localStorage.token = res.data.token;
              this.getMenu();
            });
        } else {
          return false;
        }
      });
    },
    getMenu() {
      this.$http.get("getMenu").then(res => {
        // 提取菜单数组，交给本地存储
        console.log(res)
        let menu = res.data.menu;
        // 将原始数据进行本地存储
        localStorage.menu = JSON.stringify(menu);
        // 解析出路由配置表
        const _routes = generateRoutes(menu);
        // 动态加载路由配置表
        router.addRoutes(_routes);
        // 跳转到首页
        this.$router.push("/notes");
      });
    },
    openMsg() {
      // 注意这里使用了国际化
      this.$message.warning(this.$t("m.login.info"));
    }
  },
  watch: {
    isMemery(n, o) {
      if (n) {
        localStorage.userInfo = this.form.name;
        localStorage.passwordInfo = this.form.password;
      } else {
        localStorage.removeItem("userInfo");
        localStorage.removeItem("passwordInfo");
      }
    }
  }
};
</script>
<style scoped lang="scss">
.bg {
  position: relative;
  overflow: hidden;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  background-image: url("../../static/img/bg.jpg");
  background-position: -20% 10%;
  background-size: contain;
  #particles-js {
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
  }
}
.login-wrap {
  width: 330px;
  // border-radius: 10px;
  padding: 20px;
  z-index: 3;
  margin-right: -40%;
  position: relative;
  // background: rgba(70, 70, 70, 0.4);
  &::after {
    content: "";
    width:100%;
    height:100%;
    border-radius: 10px;
    position: absolute;
    left:0;
    top:0;
    background: rgba(70, 70, 70, 0.5);
    filter: blur(6px);
    z-index: -1;
  }
  .el-form-item {
    margin-bottom: 25px !important;
  }
  h3 {
    text-align: center;
    color: #ebedef;
    margin-top: 0px;
    margin-bottom: 5px;
    span {
      color: #20a0ff;
    }
  }
  form {
    margin-top: 25px;
    .el-form-item {
      margin-bottom: 15px;
    }
  }
  a {
    text-decoration: none;
    color: #1f2d3d;
  }
  button {
    width: 100%;
    font-weight: 600;
  }
}
</style>
