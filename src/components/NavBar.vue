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
    <header class="site-header" :class="{ 'scrolled': scrolled }">
        <div id="nav-content">
            <div id="logo-container">
                <Transition name="fade">
                    <img v-if="scrolled" id="logo" src="@/assets/logo.png" alt="Wibi Plein Logo">
                </Transition>
            </div>
            <nav class="main-nav">
                <ul>
                    <li><router-link to="/stalling">Stalling</router-link></li>
                    <li><router-link to="/statiegeld">Statiegeld</router-link></li>
                    <li><router-link to="/statiegeld">Over ons</router-link></li>
                    <li><router-link to="/contact">Contact</router-link></li>
                </ul>
            </nav>
        </div>
    </header>
</template>

<style scoped>
.site-header {
    width: 100%;
    height: 86px;
    display: flex;
    justify-content: center;
    position: fixed;
    top: 0;
    left: 0;
    align-items: center;
    background-color: transparent;
    transition: all 0.3s ease;
    z-index: 5;
    padding: 20px 40px;
}

.site-header.scrolled {
    background-color: rgba(255, 255, 255, 0.85);
    box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
}

.site-header::before {
    content: '';
    position: absolute;
    inset: 0;
    backdrop-filter: blur(20px);
    -webkit-backdrop-filter: blur(20px);
    z-index: -1;
    -webkit-mask-image: linear-gradient(to bottom, rgba(0, 0, 0, 1) 0%, rgba(0, 0, 0, 0.8) 40%, rgba(0, 0, 0, 0) 100%);
    mask-image: linear-gradient(to bottom, rgba(0, 0, 0, 1) 0%, rgba(0, 0, 0, 0.8) 40%, rgba(0, 0, 0, 0) 100%);
}

#nav-content {
    display: flex;
    width: 1280px;
    justify-content: space-between;
    align-items: center;
}

#logo-container {
    width: 72px;
    height: 72px;
    display: flex;
    align-items: center;
}

#logo {
    height: 72px;
    padding: 0 50%;
    transition: height 0.3s ease;
}

.fade-enter-active,
.fade-leave-active {
    transition: opacity 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
    opacity: 0;
}

.main-nav {
    ul {
        display: flex;
        gap: 64px;
        list-style: none;
        margin: 0;
        padding: 0;
    }

    a {
        text-decoration: none;
        font-size: 24px;
        color: #1B3b5b;
        font-weight: 600;
    }
}
</style>