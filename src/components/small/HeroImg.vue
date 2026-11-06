<script setup>
import { ref } from 'vue'

const borderReady = ref(false)

const onBorderFinished = () => {
    borderReady.value = true
}
</script>

<template>
    <div class="hero-right">
        <div class="industrial-square" @animationend.self="onBorderFinished">
            <div class="image-reveal-wrapper" :class="{ 'is-revealed': borderReady }">
                <div class="hero-image"></div>
            </div>
        </div>
    </div>
</template>

<style scoped lang="scss">
.hero-right {
    width: 100%;
}

.industrial-square {
    width: 100%;
    height: 812px;
    box-shadow: 0 15px 35px rgba(27, 59, 91, 0.08);
    border-radius: 16px 0 0 16px;
    overflow: hidden;
    position: relative;

    &::after {
        content: '';
        position: absolute;
        inset: 0;
        border: 2px solid #1B3b5b;
        border-right: none;
        border-radius: 16px 0 0 16px;
        pointer-events: none;
        animation: drawBorder 1.8s cubic-bezier(0.16, 1, 0.3, 1) forwards;
    }
}

.image-reveal-wrapper {
    width: 100%;
    height: 100%;
    transform: scaleX(0);
    transform-origin: left;
    opacity: 0;

    &.is-revealed {
        animation: revealImage 1.6s cubic-bezier(0.16, 1, 0.3, 1) forwards;

        .hero-image {
            animation: scaleImage 1.6s cubic-bezier(0.16, 1, 0.3, 1) forwards;
        }
    }
}

.hero-image {
    width: 100%;
    height: 100%;
    background-image: url('@/assets/hero.jpg');
    background-repeat: no-repeat;
    background-position: center center;
    background-size: cover;
    transform: scaleX(1.5);
}

@keyframes drawBorder {
    0% {
        opacity: 0;
        clip-path: inset(0 100% 100% 0);
    }

    30% {
        opacity: 0.4;
    }

    100% {
        opacity: 1;
        clip-path: inset(0 0 0 0);
    }
}

@keyframes revealImage {
    0% {
        transform: scaleX(0);
        opacity: 0;
    }

    100% {
        transform: scaleX(1);
        opacity: 1;
    }
}

@keyframes scaleImage {
    0% {
        transform: scaleX(1.5);
    }

    100% {
        transform: scaleX(1);
    }
}

@media (max-width: 1024px) {
    .industrial-square {
        border-radius: 16px;
        height: 240px;

        &::after {
            border-right: 2px solid #1B3b5b;
            border-radius: 16px;
        }
    }
}
</style>