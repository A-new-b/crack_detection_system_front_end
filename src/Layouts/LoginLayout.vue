<template>
  <v-container fluid fill-height>
    <notification></notification>
    <v-dialog v-model="dialog" width="500">
      <v-card class="elevation-12">
        <v-toolbar dark color="primary">
          <v-toolbar-title>检测人信息</v-toolbar-title>
        </v-toolbar>
        <v-card-text>
          <v-form>
            <v-text-field
              prepend-icon="person"
              name="login"
              label="名称"
              type="text"
              :rules="rules"
              v-model="name"
            ></v-text-field>
          </v-form>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" @click="Login">确定</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-layout>
      <div style="width: 100%">
        <template v-if="screenWidth > 700">
          <v-card>
            <v-img
              src="/api/file?file=background_pc.jpg"
              lazy-src="/api/file?file=background_pc.jpg"
              width="100%"
            >
              <v-card-title
                style="
                  display: flex;
                  flex-direction: row;
                  align-items: center;
                  color: white;
                "
                class="text-h4"
              >
                连铸坯裂痕监测系统
              </v-card-title>

            </v-img>
          </v-card>
        </template>
        <template v-else>
          <v-img src="api/file?file=background_mobile.jpg" height="100%" >
            <v-card-title
              style="
                display: flex;
                flex-direction: row;
                align-items: center;
                color: white;
              "
              class="text-h4"
            >
              <div>连铸坯裂痕监测系统</div>
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
            margin-bottom: 5%;
          "
        >
          <v-btn
            outlined
            color="black"
            style="font-size: large"
            @click="confirm"
          >
            开始查看
          </v-btn>
        </div>
        <!--        <div style="color: white">Copyright © 2020 绝对能行小组</div>-->
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
      dialog: false,
      //对话框信息部分
      rules: [(value) => !!value || "不许为空"],
      name: "",
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
    confirm() {
      this.dialog = true;
    },
    Login() {
      if (this.name !== "") {
        localStorage.setItem("Access-Token", this.name);
        // localStorage.setItem("permission", "3");
        this.$router.push("/home");
        notify("success", "欢迎登录，" + this.name);
      } else {
        notify("error", "请输入名字");
      }
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
