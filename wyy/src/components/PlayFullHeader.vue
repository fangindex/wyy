<template>
  <div class="PlayFullHeader">
    <div class="left" @click="$emit('play-isShowPlayBar', isShowPlayBar)">
      <svg
        t="1608196080054"
        class="icon"
        viewBox="0 0 1024 1024"
        version="1.1"
        xmlns="http://www.w3.org/2000/svg"
        p-id="7432"
        width="200"
        height="200"
      >
        <path
          d="M184.858282 515.110475l44.319746-44.319746 500.551575 500.551575-44.319746 44.321552z"
          p-id="7433"
          fill="#b7c7ce"
        ></path>
        <path
          d="M184.86912 515.067123L685.420695 14.515548l44.321552 44.319746-500.553381 500.553381z"
          p-id="7434"
          fill="#b7c7ce"
        ></path>
      </svg>
    </div>
    <div class="center">
      <div class="big">{{ currentMusic.name }}</div>
      <div
        v-for="(item, index) in currentMusic.song.artists"
        :key="index"
        class="small"
      >
        <span>{{ item.name }}</span>
        <span
          v-if="index == currentMusic.song.artists.length - 1 ? false : true"
          >/</span
        >
      </div>
      <div class="fh">></div>
    </div>
    <div class="right">
      <svg
        t="1608196216374"
        class="icon"
        viewBox="0 0 1024 1024"
        version="1.1"
        xmlns="http://www.w3.org/2000/svg"
        p-id="8508"
        width="200"
        height="200"
      >
        <path
          d="M794.7 461.3c-9 0-16 7.8-14.9 16.7 1.4 11.2 2.1 22.5 2.1 34 0 35.1-6.7 69.3-19.8 101.6-13.6 33.5-33.5 63.5-59.3 89.3s-55.8 45.7-89.3 59.3C581.2 775.4 547 782 512 782c-35.1 0-69.3-6.6-101.6-19.8-33.5-13.6-63.5-33.5-89.3-59.3-25.8-25.8-45.7-55.8-59.3-89.3C248.6 581.3 242 547.1 242 512s6.6-69.3 19.8-101.6c13.6-33.5 33.5-63.5 59.3-89.3 25.8-25.8 55.8-45.7 89.3-59.3C442.7 248.7 476.9 242 512 242c14.3 0 28.5 1.1 42.5 3.3 9.1 1.4 17.2-5.6 17.2-14.8 0-7.3-5.3-13.7-12.6-14.8-91.5-14.5-188.4 13.5-258.9 83.8-117.4 117-117.5 307.4-0.3 424.6s307.1 117.2 424.3 0c68.2-68.2 96.7-161 85.5-249.8-1-7.5-7.4-13-15-13z"
          fill="#b7c7ce"
          p-id="8509"
        ></path>
        <path
          d="M776.7 233.3h-141c-8.3 0-15 6.7-15 15s6.7 15 15 15h104.4L517.6 485.7c-5.9 5.9-5.9 15.4 0 21.2 5.9 5.9 15.4 5.9 21.2 0l222.8-222.8v104.2c0 8.3 6.7 15 15 15s15-6.7 15-15v-140c0.1-8.3-6.7-15-14.9-15z"
          fill="#b7c7ce"
          p-id="8510"
        ></path>
      </svg>
    </div>
  </div>
</template>

<script>
export default {
  props: ["currentMusic", "isShowPlayBar"],
};
</script>

<style lang="less">
.PlayFullHeader {
  position: relative;
  z-index: 4;
  padding-top: 3vh;
  display: flex;
  .left {
    svg {
      width: 28px;
      height: 45px;
      margin-left: 20px;
      transform: translateY(3px);
    }
  }
  .center {
    flex: 1;
    padding: 5px;
    text-align: center;
    display: -webkit-box;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 1;
    overflow: hidden;
    text-overflow: ellipsis;
    .big {
      font-weight: bolder;
      overflow: hidden;
      font-size: 20px;
    }
    .small {
      font-size: 13px;
      display: inline-block;
    }
    .fh {
      display: inline-block;
      box-sizing: border-box;
      transform: translateY(1px);
    }
  }
  .right {
    text-align: right;
    padding-top: 3px;
    svg {
      width: 45px;
      height: 45px;
      margin-right: 20px;
    }
  }
}
</style>