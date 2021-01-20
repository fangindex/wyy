<template>
  <div class="PlayFullChart">
    <ul
      v-if="lyrucs"
      class="lyric"
      @touchstart="touchstart"
      @touchmove="touchmove"
      @touchend="touchend"
      :style="{ marginTop: -40 * (i - 1) + 100 + 'px' }"
      ref="lyrics"
    >
      <div class="xian" :class="{ active: bool }">
        <svg
          t="1608742939666"
          class="icon xian-icon"
          viewBox="0 0 1024 1024"
          version="1.1"
          xmlns="http://www.w3.org/2000/svg"
          p-id="2696"
          width="18"
          height="18"
        >
          <path
            d="M314.176 162.88l526.976 316.16a38.4 38.4 0 0 1 0 65.92l-526.976 316.16A38.4 38.4 0 0 1 256 828.16V195.84a38.4 38.4 0 0 1 58.176-32.96z"
            fill="#ffffff"
            p-id="2697"
          ></path>
        </svg>
        <div>
          {{ newMinute >= 10 ? newMinute : "0" + newMinute }}:{{
            newSecond >= 10 ? newSecond : "0" + newSecond
          }}
        </div>
      </div>
      <li
        v-for="(item, index) in lyrucs"
        :key="index"
        :style="{ color: index == i - 1 ? '#ec4243' : '#b7c7ce' }"
      >
        {{ item.text || "......" }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  props: ["lyrucs", "i", "currentTime", "jiaCurrentTime"],
  data: function () {
    return {
      bool: false,
      newMinute: 0,
      newSecond: 0,
    };
  },
  methods: {
    ct(param) {
      if (this.scrollY < 0) {
        return param[
          this.i - 1 + this.scrollY >= 0 ? this.i - 1 + this.scrollY : 0
        ];
      } else {
        return param[
          this.i - 1 + this.scrollY <= this.lyrucs.length - 1
            ? this.i - 1 + this.scrollY
            : this.lyrucs.length - 1
        ];
      }
    },
    touchstart(e) {
      this.y = e.touches[0].clientY;
      console.log(this.y);
    },
    touchmove(e) {
      e.stopPropagation();
      this.bool = true;
      this.$emit("touBool", this.bool);
      this.scrollY = Math.floor((this.y - e.touches[0].clientY) / 40);
      let lyrics = this.$refs.lyrics;
      lyrics.style.marginTop = -40 * (this.i - 1 + this.scrollY) + 100 + "px";
      let lis = lyrics.querySelectorAll("li");
      for (var j = 0; j < lis.length; j++) {
        lis[j].style.color = "#b7c7ce";
      }
      this.ct(lis).style.color = "#ec4243";
      if (this.scrollY) {
        this.$emit("toucts", this.ct(this.lyrucs).time);
        this.newMinute = Math.floor(this.jiaCurrentTime / 60);
        this.newSecond = Math.floor(this.jiaCurrentTime % 60);
      }
    },
    touchend() {
      if (this.scrollY) {
        this.$emit("touct", this.ct(this.lyrucs).time);
        this.bool = false;
        this.$emit("touBool", this.bool);
      }
    },
  },
};
</script>

<style lang="less">
.PlayFullChart {
  font-size: 18px;
  line-height: 40px;
  margin-top: 100px;
  position: relative;
  margin: 5vh 0;
  text-align: center;
  height: 55vh;
  overflow: hidden;
  color: #b7c7ce;
  z-index: 4;
  position: relative;
  > ul {
    transition: all 0.5s;
    .xian {
      .xian-icon {
        position: absolute;
        left: -17px;
        top: -8px;
      }
      > div {
        font-size: 14px;
        position: absolute;
        right: -40px;
        top: -19px;
      }
      position: absolute;
      top: 119px;
      left: 12vw;
      width: 75vw;
      height: 2px;
      background-image: repeating-linear-gradient(
        to right,
        #fff,
        transparent,
        transparent,
        transparent,
        transparent,
        #fff
      );
      filter: brightness(0.35);
      z-index: -1;
      display: none;
    }
    .active {
      display: block;
    }
    > li {
      overflow: hidden;
      white-space: nowrap;
      text-overflow: ellipsis;
      transition: all 0.5s;
      padding: 0 20px;
    }
  }
}
</style>