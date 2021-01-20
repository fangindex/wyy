<template>
  <li class="Hots" @click="$emit('translate-music', item)">
    <div class="top">{{ item.name }}</div>
    <span class="left"></span>
    <span
      v-for="(artist, index) in item.song.artists"
      :key="index"
      class="bottom"
    >
      <span>{{ artist.name }}</span>
      <!-- <span v-if="pd">-</span> -->
      <span class="center">/</span>
    </span>
    <span>{{ item.name }}</span>
    <svg
      v-if="currentMusic && currentMusic.id === item.id"
      :class="{ paused: !pause }"
      t="1608105540664"
      class="icon right dd"
      viewBox="0 0 1024 1024"
      version="1.1"
      xmlns="http://www.w3.org/2000/svg"
      p-id="8957"
      width="200"
      height="200"
    >
      <path
        d="M350.070803 882.280288V216.782468c0-5.212522 3.653637-9.645602 8.76873-10.619905l482.669819-95.530431a10.814766 10.814766 0 0 1 12.714657 8.525153l0.194861 2.094752v582.438469a48.715161 48.715161 0 1 0 97.430322 0V121.203322a108.245089 108.245089 0 0 0-129.241323-106.199052l-482.718534 95.481716a108.245089 108.245089 0 0 0-87.200139 106.199052v665.59525a48.715161 48.715161 0 1 0 97.430323 0z"
        fill="#E01313"
        p-id="8958"
      ></path>
      <path
        d="M69.565111 948.194845a121.787904 146.145484 70 1 0 274.663666-99.969398 121.787904 146.145484 70 1 0-274.663666 99.969398Z"
        fill="#E01313"
        p-id="8959"
      ></path>
      <path
        d="M669.085941 777.256282a121.787904 146.145484 70 1 0 274.663666-99.969399 121.787904 146.145484 70 1 0-274.663666 99.969399Z"
        fill="#E01313"
        p-id="8960"
      ></path>
      <path
        d="M267.742181 364.584268l15.1017 71.465142L945.02907 296.188181l-15.1017-71.513857z"
        fill="#E01313"
        p-id="8961"
      ></path>
    </svg>
    <svg
      v-else
      t="1608105474487"
      class="icon right"
      viewBox="0 0 1024 1024"
      version="1.1"
      xmlns="http://www.w3.org/2000/svg"
      p-id="4967"
      width="200"
      height="200"
    >
      <path
        d="M512 512m-512 0a512 512 0 1 0 1024 0 512 512 0 1 0-1024 0Z"
        fill="#E73828"
        p-id="4968"
      ></path>
      <path
        d="M846.185477 516.409392L343.004261 806.918684a1.415647 1.415647 0 0 1-2.123471-1.160367V224.670112a1.415647 1.415647 0 0 1 2.123471-1.229989l503.181216 290.509292a1.42725 1.42725 0 0 1 0 2.459977z"
        fill="#E73828"
        p-id="4969"
      ></path>
      <path
        d="M382.665579 794.792856a59.178678 59.178678 0 0 1-58.935002-59.016227V294.570574a58.935001 58.935001 0 0 1 88.36189-51.056115L794.189466 464.146496a58.911794 58.911794 0 0 1 0 102.112229L412.092467 786.809537a58.818965 58.818965 0 0 1-29.426888 7.983319z m0-524.891668a24.739008 24.739008 0 0 0-24.657783 24.669386v441.206055a24.657783 24.657783 0 0 0 36.980872 21.339136l382.062189-220.597226a24.622972 24.622972 0 0 0 0-42.678271L394.988668 273.243042a24.599764 24.599764 0 0 0-12.323089-3.341854z"
        fill="#FFFFFF"
        p-id="4970"
      ></path>
      <path
        d="M512 512m-512 0a512 512 0 1 0 1024 0 512 512 0 1 0-1024 0Z"
        fill="#E73828"
        p-id="4971"
      ></path>
      <path
        d="M846.185477 516.409392L343.004261 806.918684a1.415647 1.415647 0 0 1-2.123471-1.160367V224.670112a1.415647 1.415647 0 0 1 2.123471-1.229989l503.181216 290.509292a1.42725 1.42725 0 0 1 0 2.459977z"
        fill="#E73828"
        p-id="4972"
      ></path>
      <path
        d="M382.665579 811.966277a76.38691 76.38691 0 0 1-76.073611-76.189648V294.570574a76.073611 76.073611 0 0 1 114.110416-65.862388l382.062189 220.585622a76.073611 76.073611 0 0 1 0 131.771191l-382.062189 220.585622a75.992385 75.992385 0 0 1-38.036805 10.315656z m0-524.949687a7.484362 7.484362 0 0 0-3.759587 1.07914 7.147856 7.147856 0 0 0-3.759586 6.486447v441.194452a7.530777 7.530777 0 0 0 11.267156 6.509655l382.073792-220.60883a7.495966 7.495966 0 0 0 0-12.996101l-382.073792-220.585623a7.298704 7.298704 0 0 0-3.747983-1.07914z"
        fill="#FFFFFF"
        p-id="4973"
      ></path>
    </svg>
  </li>
</template>

<script>
export default {
  props: ["item", "currentMusic", "pause"],
};
</script>

<style lang="less">
.Hots {
  border-bottom: 1px solid #eeeeee;
  padding: 10px;
  position: relative;

  .top {
    font-size: 18px;
  }
  .left {
    display: inline-block;
    background: url("https://s3.music.126.net/mobile-new/img/index_icon_2x.png?5207a28c3767992ca4bb6d4887c74880=")
      no-repeat 0 0;
    background-size: 250px 130px;
    width: 21px;
    height: 14px;
    vertical-align: middle;
  }
  span {
    font-size: 13px;
    color: #888;
  }
  span.center {
    margin: 0 3px;
  }
  .right {
    position: absolute;
    top: 30%;
    right: 10px;
    // transform: translateY(-50%);

    width: 25px;
    height: 25px;
  }
  .dd {
    animation: playing 2s linear infinite;
    transform-origin: center center;
    // position: absolute;
    // top: 40%;
  }
  .paused {
    animation-play-state: paused;
  }
}
@keyframes playing {
  from {
    // height: 50px;
    transform: rotate(0deg);
  }
  to {
    // height: 10px;
    transform: rotate(360deg);
  }
}
</style>