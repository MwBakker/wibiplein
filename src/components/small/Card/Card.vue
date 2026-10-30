<script setup>
defineProps({
  title: {
    type: String,
    required: true
  },
  description: {
    type: String,
    required: true
  },
  features: {
    type: Array,
    default: () => []
  },
  iconSrc: {
    type: String,
    required: true
  }
})
</script>

<template>
  <div class="card-widget">
    <div class="icon-header">
      <img :src="iconSrc" :alt="title" class="card-icon" />
    </div>
    <h3>{{ title }}</h3>
    <p class="description">{{ description }}</p>
    <ul v-if="features.length" class="feature-list">
      <li v-for="(feature, index) in features" :key="index">
        <span class="bullet">•</span> {{ feature }}
      </li>
    </ul>
  </div>
</template>