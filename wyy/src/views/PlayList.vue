<template>
  <div
    class="playlists"
    v-if="songs"
    :class="{ RecommendActive: currentMusic }"
  >
    <div class="playlists-top" v-show="!$root.loading">
      <svg
        @click="$router.back()"
        t="1608797168618"
        class="icon playlist-icon"
        viewBox="0 0 1024 1024"
        version="1.1"
        xmlns="http://www.w3.org/2000/svg"
        p-id="6792"
        width="30"
        height="30"
      >
        <path
          d="M473.472 201.92c13.888 13.952 14.08 36.416 0.384 50.56l-0.384 0.384L250.24 475.968h609.728a35.968 35.968 0 1 1 0 72H250.944l222.464 222.464c13.888 13.952 14.08 36.416 0.384 50.56l-0.384 0.384a36.032 36.032 0 0 1-50.56 0.384L139.392 538.24a36.224 36.224 0 0 1-4.864-5.632l3.84 4.48a36.096 36.096 0 0 1-8.384-13.12 35.648 35.648 0 0 1 8-37.312l284.608-284.736c14.08-14.08 36.864-14.08 50.944 0z"
          fill="#888"
          p-id="6793"
        ></path>
      </svg>
      <div class="zzc-two"></div>
      <div
        class="zzc"
        :style="{ backgroundImage: 'url(' + songs.coverImgUrl + ')' }"
      ></div>
      <div class="centers">
        <div class="hb">
          <span class="bfl">{{ songs.playCount | formatPlayCount }}</span>
          <div class="gd">歌单</div>
          <img :src="songs.coverImgUrl" alt="" />
        </div>
        <div class="wb">
          <div class="top">
            {{ songs.name }}
          </div>
          <div class="bottom">
            <div class="left">
              <img :src="songs.creator.avatarUrl" alt="" />
            </div>
            <div class="right">{{ songs.creator.nickname }}</div>
          </div>
        </div>
      </div>
    </div>
    <div class="loading" v-show="$root.loading">
      <img
        src="data:image/gif;base64,R0lGODlhKAAoAIABANM6Mf///yH/C05FVFNDQVBFMi4wAwEAAAAh/wtYTVAgRGF0YVhNUDw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuNi1jMDY3IDc5LjE1Nzc0NywgMjAxNS8wMy8zMC0yMzo0MDo0MiAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENDIDIwMTUgKFdpbmRvd3MpIiB4bXBNTTpJbnN0YW5jZUlEPSJ4bXAuaWlkOkI2QURFODRFRkZBRTExRTU4NTg0QTdFMUQ4QkI2MTI1IiB4bXBNTTpEb2N1bWVudElEPSJ4bXAuZGlkOkI2QURFODRGRkZBRTExRTU4NTg0QTdFMUQ4QkI2MTI1Ij4gPHhtcE1NOkRlcml2ZWRGcm9tIHN0UmVmOmluc3RhbmNlSUQ9InhtcC5paWQ6QjZBREU4NENGRkFFMTFFNTg1ODRBN0UxRDhCQjYxMjUiIHN0UmVmOmRvY3VtZW50SUQ9InhtcC5kaWQ6QjZBREU4NERGRkFFMTFFNTg1ODRBN0UxRDhCQjYxMjUiLz4gPC9yZGY6RGVzY3JpcHRpb24+IDwvcmRmOlJERj4gPC94OnhtcG1ldGE+IDw/eHBhY2tldCBlbmQ9InIiPz4B//79/Pv6+fj39vX08/Lx8O/u7ezr6uno5+bl5OPi4eDf3t3c29rZ2NfW1dTT0tHQz87NzMvKycjHxsXEw8LBwL++vby7urm4t7a1tLOysbCvrq2sq6qpqKempaSjoqGgn56dnJuamZiXlpWUk5KRkI+OjYyLiomIh4aFhIOCgYB/fn18e3p5eHd2dXRzcnFwb25tbGtqaWhnZmVkY2JhYF9eXVxbWllYV1ZVVFNSUVBPTk1MS0pJSEdGRURDQkFAPz49PDs6OTg3NjU0MzIxMC8uLSwrKikoJyYlJCMiISAfHh0cGxoZGBcWFRQTEhEQDw4NDAsKCQgHBgUEAwIBAAAh+QQJCgABACwAAAAAKAAoAAACeIyPqcvtD6OctNoD8rUZ7Np9VChKZAmdqKOuTOsqcIzMtGHfuaxxfbRrBGu/UfExXCRxxwRsGdg9m0IqpgmVYq1KbnTrMXmnYeAYzCtf1em2E11lf+VkFpxIP+f37td93dfF9zboVwhIaHfItJjoiBd4IzlJWalQAAAh+QQJCgABACwAAAAAKAAoAAACfIyPqcvtD6OctNqLs94WeB55AAiJ5GOeTaoubJu8nBzQGm0zuYF/0l7zIYAxocvIQzqAvVHROVRGoYemgqm0PpfZLjX53YqnV2nVWw5j1ejxkQ1Pc+Nu8FxeD4bJea2uzRf4hidotwJ4RvenmEg42IfoaFioB2N5iZmZUAAAIfkECQoAAQAsAAAAACgAKAAAAn+Mj6nL7Q+jnLRaCPK1GezafVTIaRIJmhE6qpg7sY98wg692g3+egnNm9mAup6C6EslD8hL8zcsGp4I6sI6jS6PWu42EAR3od8wVlyWor1s8m1chV/l2fQ3bm/j33m3n3F2tvfHREdYp1d4p5iIePgYqBbZB2goKIKZqbnJ+VAAACH5BAUKAAEALAAAAAAoACgAAAJ8jI+py+0PIwRUWkbB3Sjz731bKFpkGZ1mJaktm8KX29CT/Ng57ug9XwPeNC/iSLjwBY1DEFKhxDwTLl/0UH1eDVkmlNf9eqng8thqFquX606aTT6/pfJ6OznH5u/cfQBtF8cnSOgWSDcYBmeYqOWniFiod4hSaXmJmWlRAAA7"
        alt=""
      />
    </div>

    <div v-show="!$root.loading">
      <div class="lb">歌曲列表</div>
      <SearchItem
        v-for="(item, index) in newSong"
        :key="index"
        :item="item"
        :index="index"
        :pause="pause"
        :currentMusic="currentMusic"
        @translate-music="$emit('translate-music', { item, index })"
        @translate-index="$emit('translate-index', newSong)"
      ></SearchItem>
    </div>
    <div v-if="jcpl" v-show="!$root.loading">
      <div class="lb">精彩评论</div>
      <div v-for="(item, index) in jcpl" :key="index" class="jcpl">
        <img :src="item.user.avatarUrl" alt="" />
        <span class="mz">{{ item.user.nickname }}</span>
        <span class="sj"
          >{{new Date().getDate() > new Date().getDate(item.time) ?new Date().getFullYear(item.time) + '年' + new Date().getMonth(item.time) + '月' + new Date().getDate(item.time) +'日' :new Date(item.time).getHours() + ':'+
            new Date(item.time).getMinutes()
          }}</span
        >

        <span class="text">{{ item.content }}</span>
        <div class="dz">
          <span>{{ item.likedCount }}</span>
          <svg
            t="1609142098012"
            class="icon"
            viewBox="0 0 1024 1024"
            version="1.1"
            xmlns="http://www.w3.org/2000/svg"
            p-id="2534"
            width="28"
            height="28"
          >
            <path
              d="M762.52670389 406.10600305c-23.8305889-6.23213819-79.86302487-6.11040741-161.86469234-8.30989947 3.91089625-17.90336642 4.76659515-34.03446009 4.76659515-62.63167598C605.48923342 266.72824647 555.62830892 206.4796294 511.5105309 206.4796294c-31.16250494 0-56.82621608 25.47995336-57.19315877 56.76593945-0.42725115 38.37323125-12.22081482 104.67064245-76.2578845 138.33898732-4.70513466 2.44416296-18.08655134 9.04327567-20.04200901 9.95983677l0.97742331 0.85569892a60.49303338 60.49303338 0 0 0-38.12856035-15.27601854H263.67304277A57.25464475 57.25464475 0 0 0 206.4796294 454.31775403V759.47150019c0 31.52944764 25.66371113 57.19315877 57.19315877 57.19315878h57.19379528a56.52126856 56.52126856 0 0 0 50.89956644-32.93471405l0.85569891 0.24465498 4.58280556 1.22148317c0.30552037 0 0.48871294 0.0614621 0.8551006 0.1837896 10.99873334 2.74968334 32.14112489 7.75970821 77.29665376 18.1474008 9.77665186 2.20010311 60.85940322 13.13737594 113.89799416 13.13737595h104.24355046c31.77411855 0 55.29918707-11.85380848 68.25325079-36.78414342 12.71015662-22.12037498 74.91359487-140.84489085 74.85249079-308.20894987 0-24.0752598-16.68128494-54.81048179-54.07710559-65.50356746ZM339.93092728 759.47150019c0 10.51002804-8.55457038 19.06459841-19.06517128 19.06459842H263.67367928a19.06459841 19.06459841 0 0 1-19.06402557-19.06459843V454.31775403c0-10.50939155 8.55457038-19.06402558 19.06402557-19.06402558h57.19379526c10.57087751 0 19.06459841 8.55457038 19.06459844 19.06402558V759.47150019ZM778.04713872 483.6477096c-7.94352964 191.56127238-69.9030608 278.26795355-69.9030608 278.26795356-5.80488704 10.38769261-15.03134765 16.62043546-34.70698679 16.62043546H569.25451745c-52.36631882 0-104.30465452-11.85444499-105.64894416-12.22081483-79.19088007-18.20888679-83.3453206-19.61415319-88.29538709-21.0200561l2.74968332-280.16217984c0-28.96294924 0.61104074-21.63109684 0-21.2640905C465.19427923 407.81755363 491.71344739 328.68777763 492.44669628 263.73414685A19.1254479 19.1254479 0 0 1 511.51116741 244.66967572c20.16434446 0 55.78789237 40.45102436 55.78789237 90.55623786 0 45.21701484-1.83312222 52.97672306-17.65933201 100.0884734 190.64471125 0 189.36152571 2.74968334 206.16514607 7.14866746 20.89708416 5.98867664 22.60850743 23.22012101 22.60850743 29.20819299 0 6.53826323-0.1223278 5.62109745-0.36638577 11.97614393Z"
              fill="#888888"
              p-id="2535"
            ></path>
          </svg>
        </div>
      </div>
    </div>
    <div v-if="zxpl" v-show="!$root.loading">
      <div class="lb">最新评论</div>
      <div v-for="(item, index) in zxpl" :key="index" class="zxpl">
        <img :src="item.user.avatarUrl" alt="" />
        <span class="mz">{{ item.user.nickname }}</span>
        <span class="sj"
          >{{new Date().getFullYear(item.time) + '年' + new Date().getMonth(item.time) + '月' + new Date().getDate(item.time) +'日'
          }}</span
        >

        <span class="text">{{ item.content }}</span>
        <div class="dz">
          <span>{{ item.likedCount }}</span>
          <svg
            t="1609142098012"
            class="icon"
            viewBox="0 0 1024 1024"
            version="1.1"
            xmlns="http://www.w3.org/2000/svg"
            p-id="2534"
            width="28"
            height="28"
          >
            <path
              d="M762.52670389 406.10600305c-23.8305889-6.23213819-79.86302487-6.11040741-161.86469234-8.30989947 3.91089625-17.90336642 4.76659515-34.03446009 4.76659515-62.63167598C605.48923342 266.72824647 555.62830892 206.4796294 511.5105309 206.4796294c-31.16250494 0-56.82621608 25.47995336-57.19315877 56.76593945-0.42725115 38.37323125-12.22081482 104.67064245-76.2578845 138.33898732-4.70513466 2.44416296-18.08655134 9.04327567-20.04200901 9.95983677l0.97742331 0.85569892a60.49303338 60.49303338 0 0 0-38.12856035-15.27601854H263.67304277A57.25464475 57.25464475 0 0 0 206.4796294 454.31775403V759.47150019c0 31.52944764 25.66371113 57.19315877 57.19315877 57.19315878h57.19379528a56.52126856 56.52126856 0 0 0 50.89956644-32.93471405l0.85569891 0.24465498 4.58280556 1.22148317c0.30552037 0 0.48871294 0.0614621 0.8551006 0.1837896 10.99873334 2.74968334 32.14112489 7.75970821 77.29665376 18.1474008 9.77665186 2.20010311 60.85940322 13.13737594 113.89799416 13.13737595h104.24355046c31.77411855 0 55.29918707-11.85380848 68.25325079-36.78414342 12.71015662-22.12037498 74.91359487-140.84489085 74.85249079-308.20894987 0-24.0752598-16.68128494-54.81048179-54.07710559-65.50356746ZM339.93092728 759.47150019c0 10.51002804-8.55457038 19.06459841-19.06517128 19.06459842H263.67367928a19.06459841 19.06459841 0 0 1-19.06402557-19.06459843V454.31775403c0-10.50939155 8.55457038-19.06402558 19.06402557-19.06402558h57.19379526c10.57087751 0 19.06459841 8.55457038 19.06459844 19.06402558V759.47150019ZM778.04713872 483.6477096c-7.94352964 191.56127238-69.9030608 278.26795355-69.9030608 278.26795356-5.80488704 10.38769261-15.03134765 16.62043546-34.70698679 16.62043546H569.25451745c-52.36631882 0-104.30465452-11.85444499-105.64894416-12.22081483-79.19088007-18.20888679-83.3453206-19.61415319-88.29538709-21.0200561l2.74968332-280.16217984c0-28.96294924 0.61104074-21.63109684 0-21.2640905C465.19427923 407.81755363 491.71344739 328.68777763 492.44669628 263.73414685A19.1254479 19.1254479 0 0 1 511.51116741 244.66967572c20.16434446 0 55.78789237 40.45102436 55.78789237 90.55623786 0 45.21701484-1.83312222 52.97672306-17.65933201 100.0884734 190.64471125 0 189.36152571 2.74968334 206.16514607 7.14866746 20.89708416 5.98867664 22.60850743 23.22012101 22.60850743 29.20819299 0 6.53826323-0.1223278 5.62109745-0.36638577 11.97614393Z"
              fill="#888888"
              p-id="2535"
            ></path>
          </svg>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import SearchItem from "@/components/SearchItem.vue";

export default {
  components: {
    SearchItem,
  },
  props: ["item", "index", "pause", "currentMusic"],
  data: function () {
    return {
      songNames: [],
      newSong: [],
      songs: null,
      count: 0,
      jcpl: null,
      zxpl: null,
    };
  },
  filters: {
    formatPlayCount: function (value) {
      if (value >= 100000000) {
        return (value / 100000000).toFixed(1) + "亿";
      }
      return (value / 10000).toFixed(1) + "万";
    },
  },
  activated() {
    this.$root.loading = true;
    this.newSong = [];
    var name = this.$route.query.id;
    var srtSongName = "";
    // console.log(name);
    this.axios.get("/comment/playlist?id=" + name).then((response) => {
      this.jcpl = response.data.hotComments;
      console.log(response.data.comments);
      this.zxpl = response.data.comments;
    });
    this.axios.get("/playlist/detail?id=" + name).then((response) => {
      var songName = response.data.playlist.trackIds;
      this.songs = response.data.playlist;
      // console.log(this.songs);
      for (var i = 0; i < songName.length; i++) {
        if (i < songName.length - 1) {
          srtSongName += songName[i].id + ",";
        } else {
          srtSongName += songName[i].id;
        }
      }
      this.axios.get("/song/detail?ids=" + srtSongName).then((response) => {
        this.songNames = response.data.songs;
        for (i in this.songNames) {
          var k = {};
          k.name = this.songNames[i].name;
          k.picUrl = this.songNames[i].al.picUrl;
          k.song = { artists: this.songNames[i].ar };
          k.id = this.songNames[i].id;
          this.newSong.push(k);
        }
        // console.log(this.songNames);
        // console.log(this.newSong);
        this.count += 1;
      });
    });
  },
  watch: {
    count() {
      if (this.count) {
        this.$root.loading = false;
      }
    },
  },
};
</script>

<style lang="less">
.playlists {
  .jcpl,
  .zxpl {
    position: relative;
    height: 100px;
    border-bottom: 1px solid #ccc;
    padding: 7px;
    > img {
      width: 40px;
      height: 40px;
      border-radius: 50%;
    }
    .mz {
      font-size: 16px;
      position: absolute;
      top: 5px;
      left: 55px;
    }
    .sj {
      color: #666;
      font-size: 14px;
      position: absolute;
      top: 28px;
      left: 55px;
    }
    .text {
      position: absolute;
      bottom: 10px;
      left: 55px;
      display: -webkit-box;
      -webkit-box-orient: vertical;
      -webkit-line-clamp: 2;
      overflow: hidden;
      text-overflow: ellipsis;
    }
    .dz {
      position: absolute;
      top: 10px;
      right: 10px;
      display: flex;
      > span {
        line-height: 28px;
      }
    }
  }
  .lb {
    background: #eeeff0;
    color: #666;
    font-size: 13px;
    height: 25px;
    line-height: 25px;
    padding-left: 10px;
  }
  .playlist-icon {
    position: absolute;
    top: 10px;
    left: 10px;
    z-index: 6;
  }
  .loading {
    text-align: center;
    padding-top: 50px;
  }
  width: 100vw;
  .playlists-top {
    overflow: hidden;

    width: 100vw;
    position: relative;
    height: 230px;
    .centers {
      display: flex;
      width: 100vw;
      height: 130px;
      position: relative;
      top: ceil(230px - 130px) / 2;
      left: 0;
      z-index: 5;
    }
    .top {
      display: -webkit-box;
      -webkit-box-orient: vertical;
      -webkit-line-clamp: 3;
      overflow: hidden;
      text-overflow: ellipsis;
      font-size: 18px;
      color: #fff;
      margin-right: 10px;
    }
    .bottom {
      margin-top: 20px;
      > div {
        float: left;
      }
      height: 30px;
      .right {
        font-size: 16px;
        height: 100%;
        line-height: 30px;
        color: #888;
      }
      .left {
        > img {
          width: 30px;
          height: 30px;
          border-radius: 50%;
        }
      }
    }
    .hb {
      margin: 0 10px;
      position: relative;
      width: 130px;
      height: 130px;
      .bfl {
        position: absolute;
        top: 0;
        right: 0;
        color: #fff;
        &::before {
          content: "";
          background: no-repeat
            url("data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyMiAyMCI+PHBhdGggZmlsbC1ydWxlPSJldmVub2RkIiBmaWxsPSIjMDQwMDAwIiBkPSJNMjIgMTYuNzc3YzAgMS4yMzMtMS4xMjEgMi4yMzMtMi41MDYgMi4yMzMtMS4zODQgMC0yLjUwNi0xLTIuNTA2LTIuMjMzdi0yLjU1M2MwLTEuMjM0IDEuMTIyLTIuMjMzIDIuNTA2LTIuMjMzLjE3NCAwIC4zNDMuMDE3LjUwNi4wNDZ2LTEuMzdoLS4wMzNjLjAxNy0uMjIuMDMzLS40NDEuMDMzLS42NjZhOCA4IDAgMCAwLTE2IDBjMCAuMjI1LjAxNi40NDYuMDM0LjY2Nkg0djEuMzdjLjE2My0uMDI5LjMzMy0uMDQ2LjUwNS0uMDQ2IDEuMzg0IDAgMi41MDYuOTk5IDIuNTA2IDIuMjMzdjIuNTUzYzAgMS4yMzMtMS4xMjIgMi4yMzMtMi41MDYgMi4yMzNTMiAxOC4wMTEgMiAxNi43Nzd2LTIuNTUzYzAtLjI1OC4wNTktLjUwMS4xNDgtLjczQS45ODIuOTgyIDAgMCAxIDIgMTMuMDAxdi0yLjY2N2MwLS4wMjMuMDEyLS4wNDMuMDEzLS4wNjctLjAwNC0uMDg4LS4wMTMtLjE3Ni0uMDEzLS4yNjYgMC01LjUyMyA0LjQ3Ny0xMCAxMC0xMHMxMCA0LjQ3NyAxMCAxMGMwIC4wOS0uMDA5LjE3OC0uMDE0LjI2Ni4wMDIuMDI0LjAxNC4wNDQuMDE0LjA2N3YyYS45ODguOTg4IDAgMCAxLS4zNi43NTNjLjIyNC4zMzQuMzYuNzIuMzYgMS4xMzh2Mi41NTIiIG9wYWNpdHk9Ii4xNSIvPjxwYXRoIGZpbGwtcnVsZT0iZXZlbm9kZCIgZmlsbD0iI2ZmZiIgZD0iTTIwIDE2Ljc3N2MwIDEuMjMzLTEuMTIxIDIuMjMzLTIuNTA2IDIuMjMzLTEuMzg0IDAtMi41MDYtMS0yLjUwNi0yLjIzM3YtMi41NTNjMC0xLjIzNCAxLjEyMi0yLjIzMyAyLjUwNi0yLjIzMy4xNzQgMCAuMzQzLjAxNy41MDYuMDQ2di0xLjM3aC0uMDMzYy4wMTctLjIyLjAzMy0uNDQxLjAzMy0uNjY2YTggOCAwIDAgMC0xNiAwYzAgLjIyNS4wMTYuNDQ2LjAzNC42NjZIMnYxLjM3Yy4xNjMtLjAyOS4zMzMtLjA0Ni41MDUtLjA0NiAxLjM4NCAwIDIuNTA2Ljk5OSAyLjUwNiAyLjIzM3YyLjU1M2MwIDEuMjMzLTEuMTIyIDIuMjMzLTIuNTA2IDIuMjMzUzAgMTguMDExIDAgMTYuNzc3di0yLjU1M2MwLS4yNTguMDU5LS41MDEuMTQ4LS43M0EuOTgyLjk4MiAwIDAgMSAwIDEzLjAwMXYtMi42NjdjMC0uMDIzLjAxMi0uMDQzLjAxMy0uMDY3LS4wMDQtLjA4OC0uMDEzLS4xNzYtLjAxMy0uMjY2IDAtNS41MjMgNC40NzctMTAgMTAtMTBzMTAgNC40NzcgMTAgMTBjMCAuMDktLjAwOS4xNzgtLjAxNC4yNjYuMDAyLjAyNC4wMTQuMDQ0LjAxNC4wNjd2MmEuOTg4Ljk4OCAwIDAgMS0uMzYuNzUzYy4yMjQuMzM0LjM2LjcyLjM2IDEuMTM4djIuNTUyIi8+PC9zdmc+");
          background-position: left center;
          background-size: auto 100%;
          width: 1.2em;
          height: 0.95em;
          display: inline-block;
        }
      }
      width: 130px;
      height: 130px;
      .gd {
        position: absolute;
        top: 10px;
        left: 0;
        background: rgba(217, 48, 48, 0.8);
        border-radius: 0 10px 10px 0;
        width: 40px;
        height: 20px;
        text-align: center;
        color: #fff;
        font-size: 12px;
        line-height: 20px;
      }
      > img {
        width: 130px;
        height: 130px;
        max-width: none;
      }
    }
    .zzc {
      position: absolute;
      top: 0;
      left: 0;
      filter: blur(15px) brightness(0.8);
      transform: scale(1.2);
      height: 100%;
      width: 100vw;
      position: absolute;
      background-size: 200% 100%;
      background-position: center center;
      background-repeat: no-repeat;
    }
    .zzc-two {
      position: absolute;
      top: 0;
      left: 0;
      height: 100%;
      width: 100vw;
      background: rgba(0, 0, 0, 0.3);
      z-index: 2;
    }
  }
  list-style: none;
}
</style>