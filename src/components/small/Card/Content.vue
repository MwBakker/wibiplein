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
})
</script>

<template>
    <div class="card-content">
        <h3>{{ title }}</h3>
        <p class="description">{{ description }}</p>
        <ul v-if="features.length" class="feature-list">
            <li v-for="(feature, index) in features" :key="index">
                <span class="bullet">•</span> {{ feature }}
            </li>
        </ul>
    </div>
</template>

<style scoped></style>