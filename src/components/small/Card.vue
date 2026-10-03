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
    <div class="stalling-card">
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

<style scoped>
.stalling-card {
    background-color: rgba(255, 255, 255, 0.75);
    border: 2px solid #1B3b5b;
    border-radius: 16px;
    padding: 1.5rem;
    display: flex;
    flex-direction: column;
    transition: transform 0.3s ease, border-color 0.3s ease, box-shadow 0.3s ease;
    box-shadow: 0 8px 20px rgba(27, 59, 91, 0.05);
}

.stalling-card:hover {
    transform: translateY(-4px);
    border-color: #2E8B57;
    box-shadow: 0 12px 28px rgba(46, 139, 87, 0.12);
}

.icon-header {
    width: 100%;
    height: 90px;
    display: flex;
    align-items: center;
    justify-content: center;
    margin-bottom: 1rem;
}

.card-icon {
    max-width: 100%;
    max-height: 100%;
    object-fit: contain;
}

h3 {
    font-size: 1.35rem;
    color: #1B3b5b;
    margin-bottom: 0.5rem;
    font-weight: 700;
}

.description {
    color: #4a5568;
    font-size: 0.9rem;
    line-height: 1.45;
    margin-bottom: 1rem;
}

.feature-list {
    list-style: none;
    padding: 0;
    margin: auto 0 0 0;
}

.feature-list li {
    font-size: 0.85rem;
    color: #2d3748;
    margin-bottom: 0.4rem;
    display: flex;
    align-items: flex-start;
    gap: 0.4rem;
}

.bullet {
    color: #2E8B57;
    font-weight: bold;
}
</style>