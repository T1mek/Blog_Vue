<template>
<div class="app">
  <h1>Страница с постами</h1>
      <div class="app_btns">
            <my-button
            @click="showDialog"
            >Создание поста
          </my-button>
        <my-select
            v-model="selectedSort"
            :options="sortOptions"
        />
      </div>

  <my-dialog v-model:show="dialogVisible">
    <post-from
        @create="createPost"
    />
  </my-dialog>
  <post-list
      :posts="sortedPosts"
      @delete="deletePost"
      v-if="!isLoading"

  />
  <div v-else>
    <h2 >Идет загрузка ...</h2>
  </div>
</div>
</template>

<script>
import PostFrom from "@/components/PostFrom";
import PostList from "@/components/PostList";
import axios from 'axios'

export default {
  name: 'App',
  data(){
    return{
      posts:[],
      dialogVisible : false,
      modificatorValue:'',
      isLoading:false,
      selectedSort:'',
      sortOptions:[
          {value:'title',name:'По название'},
          {value:'body',name:'По описание'},

      ]
    }
  },
  components: {
PostFrom,PostList,
  },
  methods:{
    createPost(post){
      this.posts.push(post)
      this.dialogVisible = false
    },
    deletePost(post){
      this.posts = this.posts.filter( p => p.id !== post.id)
    },
    showDialog(){
      this.dialogVisible=true
    },
    async fetchPosts(){

      try {
        this.isLoading = true
        const response = await axios.get('https://jsonplaceholder.typicode.com/posts?_limit=10')
        this.posts = response.data
      }
      catch (e) {
        console.log('Fail')
      }finally {
        this.isLoading =false
      }
    },

  },
  mounted() {
    this.fetchPosts()
  },
  computed:{
    sortedPosts(){
      return [...this.posts].sort((post1,post2) => post1[this.selectedSort]?.localeCompare(post2[this.selectedSort]))
    }
  },
  watch:{
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.app{
  padding: 20px;
}
.app_btns{
  margin: 15px 0;
  display: flex;
  justify-content: space-between;
}
</style>
