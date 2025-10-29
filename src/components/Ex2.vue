<script>
import blogPost from './subcomponents/BlogPost.vue'
import axios from 'axios'

export default { 
  components: { blogPost },
  data() {
    return {
      posts: [] // array of post objects
    }  
  },
  computed: {
    // Automatically detect backend URL
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
    // Fetch all posts when component is created
    axios.get(`${this.baseUrl}/posts`)
      .then(response => {
        this.posts = response.data
        console.log('Posts loaded:', response.data)
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
        // Send GET request to backend delete endpoint
        const response = await axios.get(`${this.baseUrl}/deletePost`, {
          params: { id }
        })
        console.log('Delete response:', response.data)

        // Remove deleted post from local posts array
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
    <h2 class="mb-4">Blog Posts</h2>

    <!-- If no posts available -->
    <div v-if="posts.length === 0" class="alert alert-info">
      No posts available.
    </div>

    <!-- Display posts -->
    <div class="row row-cols-1 row-cols-md-2 g-4">
      <div v-for="post in posts" :key="post.id" class="col">
        <blog-post 
          :subject="post.subject"
          :entry="post.entry"
          :mood="post.mood"
        >
          <!-- Delete button slot -->
          <button 
            class="btn btn-danger btn-sm"
            @click="deletePost(post.id)"
          >
            Delete
          </button>
        </blog-post>
      </div>
    </div>
  </div>
</template>

<style scoped>
.container {
  max-width: 900px;
}
</style>
