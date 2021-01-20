<template>
  <div class="Search">
    <SearchTop
      :currentMusic="currentMusic"
      @search-input="searchInput"
      :allMatch="allMatch"
      :newCount="newCount"
      @search-keyword="searchKeyword"
      :newsong="newsong"
      @translate-music="$emit('translate-music', $event)"
      @translate-index="$emit('translate-index', newsong)"
      :pause="$attrs.pause"
      @searchDelete="searchInput"
      :some="something"
      @something="something = $event"
    ></SearchTop>
    <SearchList
      :hots="hots"
      v-if="!something"
      @searchFirst="searchFirst"
      :newArr="newArr"
      @searchIndex="searchIndex"
    ></SearchList>
  </div>
</template>

<script>
import SearchList from "@/components/SearchList.vue";
import SearchTop from "@/components/SearchTop.vue";
export default {
  props: ["currentMusic"],
  components: {
    SearchList,
    SearchTop,
  },
  data() {
    return {
      hots: null,
      timer: null,
      allMatch: [],
      something: null,
      newCount: 0,
      newSongs: null,
      newsong: [],
      count: 0,
      counts: 0,
      newArr: [],
    };
  },
  created(){
    this.$root.loadings = true;
    this.axios.get("/search/hot").then((response) => {
      this.hots = response.data.result.hots;
      this.counts += 1;
    });
  },
  methods: {
    searchInput: function (n) {
      this.$root.newLoadings = true;
      this.something = n;
      this.allMatch=[];
      setTimeout(() => {
        clearTimeout(this.timer);
        if (!n) {
          return;
        } else {
          this.timer = setTimeout(() => {
            this.axios
              .get("/search/suggest?keywords=" + n + "&type=mobile")
              .then((response) => {
                this.allMatch = response.data.result.allMatch;
                this.newCount += 1;
              });
          }, 500);
        }
      }, 0);
    },
    
    searchKeyword: function (name) {
      this.$root.newShowLoading = true;
      this.axios.get("/search?keywords=" + name).then((response) => {
        this.newSongs = response.data.result.songs;
        this.newsong = [];
        for (var i = 0; i < this.newSongs.length; i++) {
          var k = {};
          k.name = this.newSongs[i].name;
          k.picUrl = this.newSongs[i].artists[0].img1v1Url;
          k.song = { artists: this.newSongs[i].artists };
          k.id = this.newSongs[i].id;
          this.newsong.push(k);
        }
        this.count += 1;
      });

      var key = name;
      if (this.newArr.indexOf(name) === -1) {
        this.newArr.unshift(name);
      }
      else{
        for (var i = 0; i < this.newArr.length; i++) {
          if (this.newArr[i] === key) {
            this.newArr.splice(i, 1);
            this.newArr.unshift(key);
            break;
          }
        }
      }
    },
    searchFirst: function (n) {
      this.searchInput(n);
      this.searchKeyword(n);
    },
    searchIndex:function (index) {
      this.newArr.splice(index, 1);
    }
  },
  watch: {
    newCount: function () {
      if (this.newCount) {
        this.$root.newLoadings = false;
      }
    },
    count: function () {
      if (this.count) {
        this.$root.newShowLoading = false;
      }
    },
    counts: function () {
      if (this.counts) {
        this.$root.loadings = false;
      }
    },
  },
};
</script>

<style lang="less">
.Search {
  margin-top: 40px;

  .loading {
    margin-top: 20px;
    text-align: center;
  }
}
</style>