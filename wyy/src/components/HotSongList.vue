<template>
  <div class="HotSongList">
    <div class="tp">
        
    </div>
    <div class="wz">
        更新日期：{{new Date().getMonth(updateTime)+1}}月{{new Date().getDate(updateTime)}}日
    </div>
  </div>
</template>

<script>
export default {
  props:["updateTime"],
}
</script>

<style lang="less">
.HotSongList{
    padding: 20px;
    background:url('https://s3.music.126.net/mobile-new/img/hot_music_bg_2x.jpg?f01a252389c26bcf016816242eaa6aee=') no-repeat;
    background-size: 100% 100%;
    width: 100vw;
    .tp{
        background: url('https://s3.music.126.net/mobile-new/img/index_icon_2x.png?5207a28c3767992ca4bb6d4887c74880=') no-repeat -17px -21px;
        width: 100%;
        height: 93px;
        background-size: 142px 86px;
    }
    .wz{
        color: #fff;
        font-size: 12px;
        padding-left: 10px;
    }
}
</style>