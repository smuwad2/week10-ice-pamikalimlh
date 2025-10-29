<script>
export default {
  props: {
    subject: String,
    entry: String,
    mood: String
  },
  computed: {
    imgLoc() {
      return `/assets/${this.mood?.toLowerCase() || 'neutral'}.png`
    }
  }
}
</script>

<template>
  <div class="card mb-3" style="width: 100%;">
    <img :src="imgLoc" class="card-img-top" :alt="mood + ' mood image'">
    <div class="card-body text-center">
      <h5 class="card-title">{{ subject }}</h5>
      <p class="card-text">{{ entry }}</p>
    </div>

    <!-- Centered slot for delete button -->
    <div class="card-footer text-center">
      <slot></slot>
    </div>
  </div>
</template>

<style scoped>
.card {
  border-radius: 8px;
}
.card-img-top {
  width: 100%;
  height: 100%;
  object-fit: cover;
}
</style>
