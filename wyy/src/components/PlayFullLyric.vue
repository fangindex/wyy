<template>
  <div class="PlayFullLyric">
    
    <div class="big" :class="{ pausedes: !pause }">
      <img
        src="https://s3.music.126.net/mobile-new/img/disc.png?d3bdd1080a72129346aa0b4b4964b75f="
        alt=""
      />
      <div class="small">
        <img :src="currentMusic.picUrl" alt="" class="p-dw" />
      </div>
      
    </div>
  </div>
</template>

<script>
export default {
  props: ["currentMusic", "pause"],
};
</script>

<style lang="less">
.PlayFullLyric {
  position: relative;
  z-index: 4;
  height: 45vh;
  width: 100vw;
  .big {
    margin: 4vh calc((100% - 260px) / 2);
    height: 260px;
    position: relative;
    animation: playing 8s linear infinite;
    > img {
      height: 100%;
      border-radius: 50%;
      box-shadow: 0px 0px 25px 5px #888;
    }
    .small {
      height: 164px;
      width: 164px;
      position: absolute;
      top: calc((260px - 164px) / 2);
      left: calc((260px - 164px) / 2);
      .p-dw {
        height: 100%;
        width: 100%;
        border-radius: 50%;
      }
    }
    
  }
  .pausedes {
    animation-play-state: paused;
  }
}
</style>