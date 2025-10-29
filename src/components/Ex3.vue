<script>
import blogPost from './subcomponents/BlogPost.vue'
import axios from 'axios'

export default { 
  components: { blogPost },
  data() {
    return {
      posts: []
    }  
  },
  computed: {
    baseUrl() {
      if (window.location.hostname === 'localhost') {
        return 'http://localhost:3000'
      } else {
        const codespace_host = window.location.hostname.replace('5173', '3000')
        return `https://${codespace_host}`
      }
    }
  },
  created() {
    // Load posts from backend
    axios.get(`${this.baseUrl}/posts`)
      .then(response => {
        this.posts = response.data
      })
      .catch(error => {
        console.error('Error loading posts:', error)
        this.posts = [{
          id: 0,
          subject: 'Error loading posts',
          entry: 'There was an error: ' + error.message,
          mood: 'neutral'
        }]
      })
  },
  methods: {
    async deletePost(id) {
      try {
        await axios.get(`${this.baseUrl}/deletePost`, { params: { id } })
        // Remove post locally after deletion
        this.posts = this.posts.filter(post => post.id !== id)
      } catch (error) {
        console.error('Error deleting post:', error)
      }
    }
  }
}
</script>

<template>
  <div class="container mt-4">
    <h2 class="mb-4 text-center">Blog Posts</h2>

    <div v-if="posts.length === 0" class="alert alert-info text-center">
      No posts available.
    </div>

    <!-- 👇 Vertical stacked layout -->
    <div class="d-flex flex-column align-items-center gap-3">
      <blog-post 
        v-for="post in posts" 
        :key="post.id"
        :subject="post.subject"
        :entry="post.entry"
        :mood="post.mood"
        class="blog-post"
      >
        <!-- Centered, blue delete button -->
         <button class="btn btn-primary" @click="deletePost(post.id)">Delete</button>
        <!-- <button 
          class="btn btn-primary btn-sm d-block mx-auto"
          @click="deletePost(post.id)"
        >
          Delete
        </button> -->
      </blog-post>
    </div>
  </div>
</template>

<style scoped>
.container {
  max-width: 700px;
  margin: 0 auto;
}

.blog-post {
  width: 100%;
  max-width: 500px;
}
</style>
