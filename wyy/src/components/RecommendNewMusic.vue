<template>
  <div>
    <BorderedTitle><slot></slot></BorderedTitle>
    <ul>
      <SongItem
        :pause="$attrs.pause"
        @translate-music="$emit('translate-music', $event)"
        @translate-index="$emit('translate-index', newSongs)"
        v-for="(song,index) in newSongs"
        :key="song.id"
        :item="song"
        :index="index"
        :currentMusic="$attrs.currentMusic"
      ></SongItem>
    </ul>
  </div>
</template>

<script>
import BorderedTitle from "@/components/BorderedTitle.vue";
import SongItem from "@/components/SongItem.vue";
export default {
  components: {
    BorderedTitle,
    SongItem,
  },
  props: {
    newSongs: {
      type: Array,
    },
  },
};
</script>

<style>

</style>