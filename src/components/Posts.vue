<template>
  <div class="hello mt-3">
    <ul class="list-unstyled">
      <li v-for="(post, index) in posts" v-bind:key="index" class="media m-3">
        <img
          :src="post.data.thumbnail"
          class="mr-3"
          alt="..."
        />
        <div class="media-body">
          <h5 class="mt-0 mb-1">
            <a 
            :href="createUrl(post.data.permalink)" 
            target="_blank"> {{post.data.title }}</a></h5>
          <p>
            <h3 class="text-danger">{{post.data.ups}} ⬆️</h3>
            
            <p>created {{
              formatDate(post.data.created_utc)}} ago by {{post.data.author}}</p>
            <span class="badge badge-pill badge-secondary">{{
              post.data.num_comments
            }} comments</span>
            <button 
            v-if="isImage(post)"
            @click="post.showImg = !post.showImg"
            type="button" 
            class="btn btn-primary">
              {{post.showImg ? 'Hide Image' : 'Show Image'}}
            </button>
            <div v-if="post.showImg">
              <img 
              width="640"
              :src="post.data.url" alt="">
            </div>
          </p>
        </div>
      </li>
    </ul>
  </div>
</template>

<script>
import {parseJSON, formatDistance} from 'date-fns'

export default {
  data() {
    return {
      posts: []
    };
  },
  mounted() {
    this.load();
  },
  methods: {
    load() {
      //fetch the posts
      const url = "https://www.reddit.com/r/rarepuppers/.json";
      fetch(url)
        .then(response => response.json())
        .then(result => {
          result.data.children.forEach(child => {
            child.showImg = false;
          })
          this.posts = result.data.children;
        });
    },
    formatDate(date) {
      return formatDistance(parseJSON(date * 1000), new Date())
    },
    createUrl(path) {
      return `http://www.reddit.com${path}`
    },
    isImage(post) {
      return post.data.url.match(/\.(jpg|png|jpeg|bpm)$/);
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
