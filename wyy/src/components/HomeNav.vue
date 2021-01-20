<template>
  <ul id="home-nav">
      <li><router-link to="/">推荐音乐</router-link></li>
      <li><router-link to="/hot">热歌榜</router-link></li>
      <li><router-link to="/search">搜索</router-link></li>
  </ul>
</template>

<script>
export default {

}
</script>

<style lang="less">
#home-nav{
    background: #fff;
    border-bottom: 1px solid #eeeeee;
    display: flex;
    list-style: none;
    height: 41px;
    li{
        flex: 1;
        text-align: center;
        a{
            font-size: 18px;
            text-decoration: none;
            line-height: 40px;
            display: inline-block;
            padding: 0 10px;
            color: #000;
            &.router-link-exact-active{
                color: #ec4243;
                border-bottom: 2px solid #ec4243;
            }
        }
    }
}
</style>