<template>
  <div id="app">
    <div id="nav">
      <Play
        @isShowPlayBar="isShowPlayBar = $event"
        @update:paused="pause = $event"
        @update:music="
          currentIndex = $event.index;
          currentMusic = $event.item;
        "
        :currentMusic="currentMusic"
        :currentIndex="currentIndex"
        :playlist="playlist"
        v-if="currentMusic"
        class="plays"
        :lyrucs="lyrucs"
      ></Play>
      <HomeNav v-if="!$route.meta.isShowNav"></HomeNav>
    </div>
    
    <keep-alive>
      <router-view
        @translate-music="
          currentMusic = $event.item;
          currentIndex = $event.index;
        "
        @translate-index="playlist = $event"
        :currentMusic="currentMusic"
        :pause="pause"
      />
    </keep-alive>
  </div>
</template>
<script>
import HomeNav from "@/components/HomeNav.vue";
import Play from "@/components/Play.vue";
export default {
  components: {
    HomeNav,
    Play,
  },
  data: function () {
    return {
      currentMusic: null,
      playlist: [],
      currentIndex: null,
      pause: null,
      isShowPlayBar: null,
      lyric: null,
      lyrucs: [],
    };
  },
  watch: {
    currentMusic: function () {
      this.axios.get("/lyric?id=" + this.currentMusic.id).then((response) => {
        this.lyric = response.data.lrc.lyric;
        // console.log(this.lyric);
        // console.log(response.data.lrc.lyric)
      });
    },
    lyric: function () {
      this.lyrucs = this.paresLyric(this.lyric);
    },
  },
  methods: {
    paresLyric: function (lyric) {
      var patt = /\[\d{2}:\d{2}\.\d{2,3}\]/gi;
      var arr = lyric
        .split("\n")
        .filter((e) => e)
        .map((str) => {
          var time = str.match(patt)[0].replace(/(\[|\])/gi, "");
          var timeArr = time.split(":");
          time = Number(timeArr[0]) * 60 + Number(timeArr[1]);
          var text = str.replace(patt, "");
          return { time, text };
        });
      return arr;
    },
  },
};
</script>
<style lang="less">
#nav {
  width: 100%;
  position: fixed;
  top: 0;
  z-index: 8;
}
.plays {
  z-index: 99;
}
.RecommendActive {
  margin-bottom: 8vh;
}
</style>
