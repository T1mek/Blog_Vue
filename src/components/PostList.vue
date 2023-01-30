<template>
  <div v-if="posts.length > 0">
    <h3>Список постов</h3>
    <transition-group name="post-list">
    <post-item
      v-for="post in posts"
      :key="post.id"
      :post="post"
      @delete="$emit('delete',post)"
    />
    </transition-group>
  </div>
  <h2 style="color: red" v-else>Постовов не найдено</h2>

</template>

<script>
import PostItem from "@/components/PostItem";
export default {
  components:{
    PostItem
  },
  name: "PostList",
  props:{
    posts:{
      type:Array,
      required:true
    }
  },
  methods:{

  }
}
</script>

<style scoped>
.post-list-item{
  display: inline-block;
  margin-top: 10px;
}
.post-list-move,
.post-list-enter-active,
.post-list-leave-active {
  transition: all 0.5s ease;
}
.post-list-enter-from,
.post-list-leave-to {
  opacity: 0;
  transform: translateX(30px);
}
</style>