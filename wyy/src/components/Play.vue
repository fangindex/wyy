<template>
  <div class="play">
    <audio
      ref="audio"
      autoplay
      :src="
        'https://music.163.com/song/media/outer/url?id=' +
        currentMusic.id +
        '.mp3'
      "
    ></audio>
    <transition
      name="custom-classes-transition"
      enter-active-class="animate__animated animate__slideInUp"
      leave-active-class="animate__animated animate__slideOutDown"
    >
      <div class="bottom" v-show="!isShowPlayBar">
        <img
          :class="{ pauseds: !pause }"
          :src="currentMusic.picUrl"
          @click="isShowPlayBar = true"
        />
        <div class="dww">
          <span class="big">{{ currentMusic.name }} - </span>
          <span
            v-for="(item, index) in currentMusic.song.artists"
            :key="index"
            class="small"
          >
            <span class="smalls">{{ item.name }}</span>
            <span
              v-if="
                index == currentMusic.song.artists.length - 1 ? false : true
              "
              >/</span
            >
          </span>
        </div>
        <canvas
          ref="pro"
          width="33"
          height="33"
          @click.stop="togglePlayState"
        ></canvas>
        <svg v-if="pause" t="1609138362233" class="icon bottom-icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="3850" width="200" height="200"><path d="M735.9 64.9c-51.2 0-96 44.7-96 95.8v702.6c0 51.1 44.8 95.8 96 95.8s96-44.7 96-95.8V160.7c0-51.1-44.8-95.8-96-95.8z m-447.8 0c-51.2 0-96 44.7-96 95.8v702.6c0 51.1 44.8 95.8 96 95.8s96-44.7 96-95.8V160.7c-0.1-51.1-44.9-95.8-96-95.8z" p-id="3851" fill="#888888"></path></svg>
        <svg v-else t="1609138572779" class="icon bottom-icon" viewBox="0 0 1024 1024" version="1.1" xmlns="http://www.w3.org/2000/svg" p-id="6792" width="200" height="200"><path d="M867.7 462L266.2 62.8c-39.9-26.5-93.2 2.1-93.2 50v798.4c0 47.9 53.3 76.5 93.2 50L867.7 562c35.7-23.7 35.7-76.3 0-100z" fill="#888888" p-id="6793"></path></svg>
      </div>
    </transition>
    <transition
      name="custom-classes-transition"
      enter-active-class="animate__animated animate__fadeIn"
      leave-active-class="animate__animated animate__fadeOut"
    >
      <div v-show="isShowPlayBar" class="playFull">
        <div class="bfms" v-show="newSwitchover === 0">单曲循环</div>
        <div class="bfms" v-show="newSwitchover === 1">列表循环</div>
        <div class="bfms" v-show="newSwitchover === 2">随机播放</div>
        <div class="bfms" v-show="newSwitchover === 3">心动模式</div>
        <PlayFullHeader
          :currentMusic="currentMusic"
          :isShowPlayBar="isShowPlayBar"
          @play-isShowPlayBar="isShowPlayBar = !$event"
        ></PlayFullHeader>
        <div
          class="zzc"
          :style="{ backgroundImage: 'url(' + currentMusic.picUrl + ')' }"
        ></div>
        <PlayFullLyric
          v-if="isBool"
          :currentMusic="currentMusic"
          :pause="pause"
          @click.native="isBool = !isBool"
        ></PlayFullLyric>
        <PlayFullChart
          v-else
          @click.native="isBool = !isBool"
          :lyrucs="lyrucs"
          :i="i"
          @touct="touct"
          @toucts="toucts"
          @touBool="bool = $event"
          :currentTime="currentTime"
          :jiaCurrentTime="jiaCurrentTime"
        ></PlayFullChart>
        <PlayFullFooter
          @translate-previous="playNext(newSwitchover==1?previous:random)"
          @translate-orderPlay="playNext(newSwitchover==1?orderPlay:random)"
          :isBools="isBool"
          :duration="duration"
          :currentTime="currentTime"
          @update:drag="$refs.audio.currentTime = $event"
          :pause="pause"
          @translate-togglePlayState="togglePlayState"
          @playSwitchover="newSwitchover = $event"
        ></PlayFullFooter>
      </div>
    </transition>
  </div>
</template>

<script>
import PlayFullHeader from "@/components/PlayFullHeader.vue";
import PlayFullLyric from "@/components/PlayFullLyric.vue";
import PlayFullChart from "@/components/PlayFullChart.vue";
import PlayFullFooter from "@/components/PlayFullFooter.vue";
export default {
  components: {
    PlayFullHeader,
    PlayFullLyric,
    PlayFullChart,
    PlayFullFooter,
  },
  props: ["currentMusic", "currentIndex", "playlist", "lyrucs"],
  data: function () {
    return {
      pause: null,
      duration: null,
      currentTime: null,
      isShowPlayBar: false,
      isBool: true,
      i: 0,
      bool: false,
      jiaCurrentTime: null,
      newSwitchover: 1,
    };
  },
  mounted() {
    this.$refs.audio.addEventListener("pause", () => {
      this.pause = false;
      // console.log(this.pause);
    });
    this.$refs.audio.addEventListener("play", () => {
      this.pause = true;
      // console.log(this.pause);
    });
    this.$refs.audio.addEventListener("durationchange", () => {
      this.duration = this.$refs.audio.duration;
      // console.log(this.duration);
    });
    this.$refs.audio.addEventListener("timeupdate", () => {
      this.currentTime = this.$refs.audio.currentTime;
      // console.log(this.currentTime);
      this.progress(this.duration, this.currentTime);
      if (this.newSwitchover === 0) {
        this.$refs.audio.loop = true;
      } 
      else{
        this.$refs.audio.loop = false;
      }
    });
    this.$refs.audio.addEventListener("ended", () => {
      
      if (this.newSwitchover === 1) {
        this.playNext(this.orderPlay);
      } else if (this.newSwitchover === 2) {
        this.playNext(this.random);
      } else {
        this.playNext(this.orderPlay);
      }
      
      
    });
  },
  watch: {
    pause: function (n) {
      this.$emit("update:paused", n);
    },
    currentTime: function () {
      if (!this.bool) {
        var a = this.lyrucs.findIndex((element) => {
          return element.time > this.currentTime;
        });
        if (a < 0) {
          this.i = this.lyrucs.length;
        } else {
          this.i = a;
        }
      }
    },
    newSwitchover: function (n, m) {
      var bfms = document.querySelectorAll(".bfms");
      bfms[n].style.opacity = "1";
      bfms[m].style.opacity = "0";
      setTimeout(() => {
        bfms[n].style.opacity = "0";
      }, 1000);
    },
  },
  methods: {
    toucts: function name(params) {
      this.jiaCurrentTime = params;
    },
    touct: function (params) {
      this.$refs.audio.currentTime = params;
    },
    progress: function (n, m) {
      let canvas = this.$refs.pro;
      let ctx = canvas.getContext("2d");

      // ctx.beginPath();
      // ctx.strokeStyle = "red";
      // ctx.lineWidth = 3 ;
      // ctx.arc(16.5,16.5,15,0,Math.PI * 2 ,false)
      // ctx.stroke();
      // ctx.closePath();

      ctx.clearRect(0, 0, 33, 33);
      ctx.beginPath();
      ctx.strokeStyle = "#888888";
      ctx.lineWidth = 2;
      ctx.arc(
        16.5,
        16.5,
        15,
        Math.PI * -0.5,
        Math.PI * ((m / n) * 2 - 0.5),
        false
      );
      ctx.stroke();
      ctx.closePath();
    },
    togglePlayState: function () {
      let audio = this.$refs.audio;
      if (!this.pause) {
        audio.play();
      } else {
        audio.pause();
      }
    },
    random: function () {
      let nextIndex = Math.floor(Math.random() * this.playlist.length);
      console.log(nextIndex,this.currentIndex)
      if (nextIndex === this.currentIndex) {
        return this.random();
      } else {
        return nextIndex;
      }
    },
    orderPlay: function () {
      let nextIndex;
      if (this.currentIndex >= this.playlist.length - 1) {
        nextIndex = 0;
      } else {
        nextIndex = this.currentIndex + 1;
      }
      return nextIndex;
    },
    previous: function () {
      let nextIndex;
      if (this.currentIndex <= 0) {
        nextIndex = this.playlist.length - 1;
      } else {
        nextIndex = this.currentIndex - 1;
      }
      return nextIndex;
    },
    playNext: function (fu) {
      let index = fu();
      this.$emit("update:music", {
        item: this.playlist[index],
        index,
      });
    },
  },
};
</script>

<style lang="less">
.play {
  position: fixed;
  bottom: -1px;
  left: 0;
  width: 100%;
  height: 9vh;
  audio {
    position: absolute;
    top: -100px;
    width: 100%;
    height: 40px;
  }
  .bottom {
    border-top: 1px solid #ccc;
    background: #fff;
    .pauseds {
      animation-play-state: paused;
    }
    height: 100%;
    position: relative;
    > img {
      margin: calc((9vh - 7vh) / 2);
      animation: playing 5s linear infinite;
      height: 7vh;
      width: 7vh;
      border-radius: 50%;
    }
    .dww {
    color: rgb(43, 42, 42);
      display: inline-block;
      position: absolute;
      top: calc((9vh - 20px) / 2);
      left: 65px;
      width: 240px;
-webkit-box-orient: vertical;
  -webkit-line-clamp: 1;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
      .big {
        font-size: 18px;
      }
      .small {
        margin-bottom: 20px;
        font-size: 12px;

        .smalls {
          width: 80px;
        }
      }
    }
    .bottom-icon {
      width: 20px;
      height: 20px;
      position: absolute;
      top: calc((9vh - 20px) / 2);
      right: 26px;
      border-radius: 50%;
    }
    canvas {
      position: absolute;
      top: calc((9vh - 33px) / 2);
      right: 20px;
      z-index: 999;
    }
  }
  .playFull {
    .bfms {
      position: absolute;
      top: 40%;
      left: 50%;
      transform: translateX(-50%);
      z-index: 99;
      background: rgba(0, 0, 0, 0.7);
      width: 90px;
      height: 28px;
      font-size: 16px;
      text-align: center;
      line-height: 28px;
      border-radius: 12px;
      color: #fff;
      opacity: 0;
      transition: all 1s;
    }
    position: fixed;
    top: 0;
    left: 0;
    width: 100vw;
    height: 100vh;
    background: rgb(43, 49, 53);
    color: #b7c7ce;
    .zzc {
      width: 100%;
      height: 100%;
      filter: blur(15px) brightness(0.5);
      position: absolute;
      background-size: 200% 100%;
      background-position: center center;
      background-repeat: no-repeat;
      top: 0;
      left: 0;
      z-index: 3;
    }
  }
}
</style>