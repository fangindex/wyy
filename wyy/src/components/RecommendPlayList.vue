<template>
  <div>
    <BorderedTitle><slot></slot></BorderedTitle>
    <ul class="playlist">
    <PlayListItem v-for="(item,index) in currentRecommends" :key="index" :item="item"></PlayListItem>

    </ul>
  </div>
</template>

<script>
import BorderedTitle from "@/components/BorderedTitle.vue";
import PlayListItem from "@/components/PlayListItem.vue";
export default {
  components: {
    BorderedTitle,
    PlayListItem
  },
  props:{
      currentRecommends:{
          type:Array,
      }
  }
};
</script>
     
<style lang="less">
.playlist{
  
    display: flex;
    flex-wrap: wrap;
}
</style>