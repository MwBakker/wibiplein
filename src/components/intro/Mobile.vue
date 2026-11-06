<script setup>
import { ref, onMounted } from 'vue'
import IntroBar from './IntroBar.vue'
import HeroImg from '../small/HeroImg.vue'

const words = "Professionele stalling & slimme statiegeldservice op één plek".split(" ")
const visibleCount = ref(0)
const showIntroBar = ref(false)
const showHeroImg = ref(false)

onMounted(() => {
    const interval = setInterval(() => {
        if (visibleCount.value < words.length) {
            visibleCount.value++
        } else {
            clearInterval(interval)
            setTimeout(() => {
                showIntroBar.value = true
            }, 300)
        }
    }, 275)
})

const onIntroBarReady = () => {
    showHeroImg.value = true
}
</script>

<template>
    <div id="intro-mobile">
        <div v-if="!showHeroImg" id="hero-placeholder"></div>
        <HeroImg v-else id="hero-fade-in" />
        <h1>
            <span v-for="(word, index) in words" :key="index" class="fade-word"
                :class="{ 'visible': index < visibleCount }">
                {{ word }}&nbsp;
            </span>
        </h1>
        <Transition name="fade-up" @after-enter="onIntroBarReady">
            <IntroBar v-if="showIntroBar" />
        </Transition>
    </div>
</template>

<style scoped lang="scss">
@use '@/assets/transitions.scss';

#intro-mobile {
    margin: 0 24px;
    height: 800px;
}

#hero-placeholder {
    height: 240px;
    width: 100%;
    background: transparent;
}

h1 {
    line-height: 1.4;
    min-height: 150px;
    padding: 56px 0;
}

.fade-word {
    opacity: 0;
    filter: blur(4px);
    transform: translateY(6px);
    display: inline-block;
    transition: opacity 0.5s ease, filter 0.5s ease, transform 0.5s ease;
}

.fade-word.visible {
    opacity: 1;
    filter: blur(0);
    transform: translateY(0);
}

#hero-fade-in {
    animation: fadeInComponent 0.6s ease forwards;
}

@keyframes fadeInComponent {
    from {
        opacity: 0;
        transform: translateY(10px);
    }

    to {
        opacity: 1;
        transform: translateY(0);
    }
}
</style>