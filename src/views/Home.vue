<template>
  <div>
    <div style="margin: 1%">
      <v-btn color="error" :disabled="errorBtn" @click="switchDialogTrigger">
        异常图像
      </v-btn>
    </div>
    <v-dialog v-model="dialogTrigger" max-width="400px">
      <v-card>
        <v-card-title>异常图像</v-card-title>
        <v-card-text>
          <!--          <img src="../assets/logo.png" />-->
          <v-img src="/api/file?file=%E5%B1%95%E7%A4%BA.jpg"></v-img>
          <v-simple-table>
            <template v-slot:default>
              <thead>
                <tr>
                  <th class="text-left">缺陷类型</th>
                  <th class="text-left">长度</th>
                  <th class="text-left">位置</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td>纵裂纹</td>
                  <td>10mm</td>
                  <td>x y</td>
                </tr>
              </tbody>
            </template>
          </v-simple-table>
        </v-card-text>
        <v-card-actions>
          <v-btn text color="primary" @click="switchDialogTrigger">
            关闭
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
    <div style="margin: 1%">
      <v-row
        style="display: flex; flex-direction: row; justify-content: center"
      >
        <v-col md="6" cols="12" sm="9" class="flex flex-column d-flex">
          <div>
            <video-player
              class="video-player-box"
              ref="videoPlayer"
              :playsinline="true"
              :options="playerOptions"
              @play="onPlayerPlay($event)"
              @ready="playerReadied"
              @timeupdate="onPlayerTimeupdate($event)"
              @pause="onPlayerPause($event)"
            ></video-player>
          </div>

        </v-col>
      </v-row>
    </div>
    <div style="margin: 1%">
      <v-row no-gutters>
        <v-col class="flex flex-column d-flex">
          <v-card class="pa-2 flex flex-column d-flex" outlined tile>
            <div style="font-weight: bold">相关信息</div>
          </v-card>
        </v-col>
      </v-row>

      <v-row no-gutters>
        <v-col md="2" cols="4" class="flex flex-column d-flex">
          <v-row no-gutters>
            <v-col class="flex flex-column d-flex">
              <v-card class="pa-2 flex flex-column d-flex" outlined tile>
                <div style="font-weight: bold">铸坯编号</div>
              </v-card>
            </v-col>
          </v-row>
        </v-col>
        <v-col md="4" cols="8" class="flex flex-column d-flex">
          <v-row no-gutters>
            <v-col class="flex flex-column d-flex">
              <v-card class="pa-2 flex flex-column d-flex" outlined tile>
                123456
              </v-card>
            </v-col>
          </v-row>
        </v-col>
        <v-col md="2" cols="4" class="flex flex-column d-flex">
          <v-row no-gutters>
            <v-col class="flex flex-column d-flex">
              <v-card class="pa-2 flex flex-column d-flex" outlined tile>
                <div style="font-weight: bold">铸坯宽度</div>
              </v-card>
            </v-col>
          </v-row>
        </v-col>
        <v-col md="4" cols="8" class="flex flex-column d-flex">
          <v-row no-gutters>
            <v-col class="flex flex-column d-flex">
              <v-card class="pa-2 flex flex-column d-flex" outlined tile>
                2.1m
              </v-card>
            </v-col>
          </v-row>
        </v-col>
      </v-row>

      <v-row no-gutters>
        <v-col md="2" cols="4" class="flex flex-column d-flex">
          <v-row no-gutters>
            <v-col class="flex flex-column d-flex">
              <v-card class="pa-2 flex flex-column d-flex" outlined tile>
                <div style="font-weight: bold">拉速</div>
              </v-card>
            </v-col>
          </v-row>
        </v-col>
        <v-col md="4" cols="8" class="flex flex-column d-flex">
          <v-row no-gutters>
            <v-col class="flex flex-column d-flex">
              <v-card class="pa-2 flex flex-column d-flex" outlined tile>
                0.8m/min
              </v-card>
            </v-col>
          </v-row>
        </v-col>
        <v-col md="2" cols="4" class="flex flex-column d-flex">
          <v-row no-gutters>
            <v-col class="flex flex-column d-flex">
              <v-card class="pa-2 flex flex-column d-flex" outlined tile>
                <div style="font-weight: bold">相机运行状态</div>
              </v-card>
            </v-col>
          </v-row>
        </v-col>
        <v-col md="4" cols="8" class="flex flex-column d-flex">
          <v-row no-gutters>
            <v-col class="flex flex-column d-flex">
              <v-card class="pa-2 flex flex-column d-flex" outlined tile>
                良好
              </v-card>
            </v-col>
          </v-row>
        </v-col>
      </v-row>

      <v-row no-gutters>
        <v-col md="2" cols="4" class="flex flex-column d-flex">
          <v-row no-gutters>
            <v-col class="flex flex-column d-flex">
              <v-card class="pa-2 flex flex-column d-flex" outlined tile>
                <div style="font-weight: bold">检测人</div>
              </v-card>
            </v-col>
          </v-row>
        </v-col>
        <v-col md="4" cols="8" class="flex flex-column d-flex">
          <v-row no-gutters>
            <v-col class="flex flex-column d-flex">
              <v-card class="pa-2 flex flex-column d-flex" outlined tile>
                {{ permission }}
              </v-card>
            </v-col>
          </v-row>
        </v-col>
        <v-col md="2" cols="4" class="flex flex-column d-flex">
          <v-row no-gutters>
            <v-col class="flex flex-column d-flex">
              <v-card class="pa-2 flex flex-column d-flex" outlined tile>
                <div style="font-weight: bold">检测时间</div>
              </v-card>
            </v-col>
          </v-row>
        </v-col>
        <v-col md="4" cols="8" class="flex flex-column d-flex">
          <v-row no-gutters>
            <v-col class="flex flex-column d-flex">
              <v-card class="pa-2 flex flex-column d-flex" outlined tile>
                {{ getTime() }}
              </v-card>
            </v-col>
          </v-row>
        </v-col>
      </v-row>
    </div>
  </div>
</template>

<script>
import { notify } from "../components/notification";
import { getCurrentTime } from "../utils/index";
export default {
  name: "Home",
  data() {
    return {
      playerOptions: {
        playbackRates: [0.7, 1.0, 1.5, 2.0], //播放速度
        autoplay: false, //如果true,浏览器准备好时开始回放。
        muted: false, // 默认情况下将会消除任何音频。
        loop: false, // 导致视频一结束就重新开始。
        preload: "auto", // 建议浏览器在<video>加载元素后是否应该开始下载视频数据。auto浏览器选择最佳行为,立即开始加载视频（如果浏览器支持）
        language: "zh-CN",
        aspectRatio: "16:9", // 将播放器置于流畅模式，并在计算播放器的动态大小时使用该值。值应该代表一个比例 - 用冒号分隔的两个数字（例如"16:9"或"4:3"）
        fluid: true, // 当true时，Video.js player将拥有流体大小。换句话说，它将按比例缩放以适应其容器。
        sources: [
          {
            type: "video/mp4",
            src: "/api/file?file=展示.mp4", //url地址
          },
        ],
        poster: "/api/file?file=展示.jpg", //你的封面地址
        // width: document.documentElement.clientWidth,
        notSupportedMessage: "此视频暂无法播放，请稍后再试", //允许覆盖Video.js无法播放媒体源时显示的默认信息。
        controlBar: {
          timeDivider: true,
          durationDisplay: true,
          remainingTimeDisplay: false,
          fullscreenToggle: true, //全屏按钮
        },
      },

      gklog: "", //视频观看时长
      pauseTrigger: [
        // [3, false],
        [5, false],
      ],
      errorBtn: true,
      dialogTrigger: false,
      //权限
      permission: "",
    };
  },
  mounted() {
    this.permission = localStorage.getItem("Access-Token");
  },
  components: {},
  methods: {
    onPlayerTimeupdate(player) {
      this.gklog = player.cache_.currentTime;
      let i = 0;
      for (i = 0; i < this.pauseTrigger.length; i++) {
        if (this.gklog >= this.pauseTrigger[i][0] && !this.pauseTrigger[i][1]) {
          this.$refs.videoPlayer.player.pause();
          this.pauseTrigger[i][1] = true;
          this.errorBtn = false;
          notify("error", "异常出现，请点击错误图像查看大图");
          break;
        }
      }
      console.log(" onPlayerTimeupdate!", this.gklog);
    },
    /* 设置视频进度 */
    playerReadied(player) {
      player.currentTime(this.gklog);
    },
    onPlayerPause() {},
    onPlayerPlay() {},
    switchDialogTrigger() {
      this.dialogTrigger = !this.dialogTrigger;
    },

    //当前时间

    getTime() {
      return getCurrentTime();
    },
  },
};
</script>
