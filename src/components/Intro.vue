<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const scrolled = ref(false)

const handleScroll = () => {
    scrolled.value = window.scrollY > 50
}

onMounted(() => {
    window.addEventListener('scroll', handleScroll)
})

onUnmounted(() => {
    window.removeEventListener('scroll', handleScroll)
})
</script>

<template>
    <section class="hero-section">
        <Transition name="slide-left" appear>
            <div class="hero-left">
                <img src="@/assets/logo.png" alt="Wibi Plein Logo" id="hero-logo" :class="{ 'scrolled': scrolled }" />
                <div class="hero-text">
                    <h1>Professionele stalling voor campers, caravans & oldtimers</h1>
                </div>
            </div>
        </Transition>
        <Transition name="slide-right" appear>
            <div class="hero-right">
                <div class="industrial-square">
                </div>
            </div>
        </Transition>
    </section>
    <IntroBar />
</template>

<style scoped>
.hero-section {
    display: flex;
    align-items: top;
    gap: 4rem;
    height: 1024px;
    width: 100vw;
    position: relative;
    left: 50%;
    transform: translateX(-50%);
}

.hero-left {
    padding-left: max(2rem, calc((100vw - 1280px) / 2 + 2rem));
}

#hero-logo {
    height: 560px;
    transition: height 0.4s cubic-bezier(0.16, 1, 0.3, 1);
}

#hero-logo.scrolled {
    height: 80px;
}

.hero-text {
    h1 {
        font-size: 2.5rem;
        color: #1B3b5b;
        line-height: 1.2;
        margin-bottom: 1rem;
    }
}

.hero-right {
    width: 100%;
}

.industrial-square {
    width: 100%;
    height: 812px;
    border: 2px solid #1B3b5b;
    border-right: none;
    border-radius: 20px 0 0 20px;
    background-color: rgba(255, 255, 255, 0.6); 
    background-image: url('@/assets/hero.jpg');
    background-repeat: no-repeat;
    background-position: center center;
    background-size: cover;

    box-shadow: 0 15px 35px rgba(27, 59, 91, 0.08);
    transition: transform 0.4s ease, border-color 0.4s ease;

    &:hover {
        border-color: #2E8B57;
    }
}

.slide-left-enter-from {
    opacity: 0;
    transform: translateX(-40px);
}

.slide-right-enter-from {
    opacity: 0;
    transform: translateX(40px);
}

.slide-left-enter-active,
.slide-right-enter-active {
    transition: all 2s cubic-bezier(0.16, 1, 0.3, 1);
}

.slide-left-enter-to,
.slide-right-enter-to {
    opacity: 1;
    transform: translateX(0);
}

@media (max-width: 1024px) {
    .hero-section {
        width: 100%;
        left: 0;
        transform: none;
        grid-template-columns: 1fr;
        text-align: center;
        min-height: auto;
        gap: 3rem;
        padding: 2rem;

        .hero-left {
            padding-left: 0;
        }

        .hero-right {
            width: 100%;
        }

        .industrial-square {
            border-right: 2px solid #1B3b5b;
            border-radius: 20px;
            height: 400px;
        }
    }
}
</style>