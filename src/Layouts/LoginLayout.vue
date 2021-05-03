<template>
  <v-container fluid fill-height>
    <notification></notification>
    <v-layout>
      <div style="width: 100%">
        <template v-if="screenWidth > 700">
          <v-card>
            <v-img src="../assets/background2.jpg">
              <v-card-title
                style="
                  display: flex;
                  flex-direction: row;
                  align-items: center;
                  color: white;
                "
                class="text-h4"
              >
                裂痕检测系统
              </v-card-title>

              <v-card-text class="backgroud">
                <v-img
                  src="../assets/nlog.png"
                  width="350px"
                  style="background: rgba(255, 255, 255, 0.2)"
                ></v-img>
              </v-card-text>
            </v-img>
          </v-card>
        </template>
        <template v-else>
          <v-img src="../assets/background3.jpg">
            <v-card-title
              style="
                display: flex;
                flex-direction: row;
                align-items: center;
                color: white;
              "
              class="text-h4"
            >
              <div>电子地图管理系统</div>
            </v-card-title>
          </v-img>
        </template>
      </div>
      <v-footer
        style="
          width: 100%;
          position: fixed;
          bottom: 0;
          display: flex;
          flex-direction: column;
          background: rgba(255, 255, 255, 0);
        "
      >
        <div
          style="
            display: flex;
            flex-direction: column;
            justify-content: center;
            width: 250px;
            background: rgba(255, 255, 255, 0.25);
            margin-bottom: 70px;
          "
        >
          <v-btn outlined color="white" style="font-size: large" @click="Login">
            开始查看
          </v-btn>
        </div>
        <div style="color: white">Copyright © 2020 绝对能行小组</div>
      </v-footer>
    </v-layout>
  </v-container>
</template>

<script>
// import { URL } from "../api/baseURL";
import notification from "../components/notification.vue";
import { notify } from "../components/notification";

export default {
  name: "LoginLayout",
  components: { notification },
  data() {
    return {
      show: false,
      username: "",
      password: "",
      messageshow: true,
      yzm: "",
      type: "",
      // img: `${URL}GetVerifyCode`,
      screenWidth: "",
      screenHeight: "",
      rules: [(value) => !!value || "不许为空"],
    };
  },
  computed: {
    width: function () {
      return this.screenWidth * 0.82;
    },
  },
  created() {
    // this.codechange();
  },
  watch: {
    $route() {
      // this.codechange();
    },
  },
  mounted() {
    this.screenWidth = document.body.clientWidth;
    this.screenHeight = document.body.clientHeight;
    window.onresize = () =>
      (() => {
        this.screenWidth = document.body.clientWidth;
        this.screenHeight = document.body.clientHeight;
      })();
  },
  methods: {
    codeChange() {
      const num = Math.ceil(Math.random() * 10); // 生成一个随机数（防止缓存）
      this.img = `${this.img}?${num}`;
    },
    Login() {
      localStorage.setItem("Access-Token", "test_token");
      // localStorage.setItem("permission", "3");
      this.$router.push("/home");
      notify("success", "欢迎使用");
    },
    changeImg() {
      return this.screenWidth / this.screenHeight + 0.04;
    },
  },
};
</script>

<style scoped>
.backgroud {
  display: flex;
  flex-direction: column;
  align-items: center;
}
.loginpanel {
  display: flex;
  flex-direction: column;
  align-items: center;
  background: rgba(255, 255, 255, 0.75);
}
</style>
