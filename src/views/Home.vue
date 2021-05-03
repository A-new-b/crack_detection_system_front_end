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
          <v-img
            src="http://0.0.0.0:8000/file?file=%E5%B1%95%E7%A4%BA.jpg"
          ></v-img>
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
    <div style="display: flex; flex-direction: column; align-items: center">
      <div style="width: 55%">
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
    </div>
  </div>
</template>

<script>
import { notify } from "../components/notification";
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
            src: "http://0.0.0.0:8000/file?file=展示.mp4", //url地址
          },
        ],
        poster: "http://0.0.0.0:8000/file?file=展示.jpg", //你的封面地址
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
    };
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
  },
};
</script>
