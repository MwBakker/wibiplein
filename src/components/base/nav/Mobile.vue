<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue';

const scrolled = ref(false);
const isMenuOpen = ref(false);

const handleScroll = () => {
    scrolled.value = window.scrollY > 50;
};

const toggleMenu = () => {
    isMenuOpen.value = !isMenuOpen.value;
};

const closeMenu = () => {
    isMenuOpen.value = false;
};

const headerHeight = computed(() => {
    if (!isMenuOpen.value) return '96px';
    return '380px';
});

onMounted(() => {
    window.addEventListener('scroll', handleScroll);
});

onUnmounted(() => {
    window.removeEventListener('scroll', handleScroll);
});
</script>

<template>
    <header class="site-header" :class="{ 'scrolled': scrolled, 'opened': isMenuOpen }"
        :style="{ height: headerHeight }">
        <div id="nav-content">
            <a href="#" @click="closeMenu">
                <img id="logo" src="@/assets/logo.png" alt="Wibi Plein Logo">
            </a>
            <button class="hamburger" @click="toggleMenu" :class="{ 'active': isMenuOpen }" aria-label="Menu openen">
                <span></span>
                <span></span>
                <span></span>
            </button>
        </div>
        <nav class="main-nav" v-if="isMenuOpen">
            <ul>
                <li><a href="#storage" @click="closeMenu">Stalling</a></li>
                <li><a href="#deposit" @click="closeMenu">Statiegeld</a></li>
                <li><a href="#contact" @click="closeMenu">Contact</a></li>
                <li><a href="#about" @click="closeMenu">Over ons</a></li>
            </ul>
        </nav>
    </header>
</template>

<style scoped>
.site-header {
    width: 100%;
    height: 84px;
    display: flex;
    flex-direction: column;
    position: fixed;
    top: 0;
    left: 0;
    background-color: transparent;
    transition: all 0.3s ease;
    z-index: 100;
    padding: 20px 24px;
    overflow: hidden;
    box-sizing: border-box;
}

.site-header.scrolled,
.site-header.opened {
    background-color: rgba(255, 255, 255, 0.95);
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
    width: 100%;
    justify-content: space-between;
    align-items: center;
    height: 46px;
}

#logo {
    padding-top: 16px;
    height: 72px;
    width: auto;
    object-fit: contain;
}

.hamburger {
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    width: 28px;
    height: 20px;
    background: transparent;
    border: none;
    cursor: pointer;
    padding: 0;
    z-index: 102;
}

.hamburger span {
    width: 100%;
    height: 3px;
    background-color: #1B3b5b;
    border-radius: 2px;
    transition: all 0.3s ease-in-out;
}

.hamburger.active span:nth-child(1) {
    transform: translateY(8.5px) rotate(45deg);
}

.hamburger.active span:nth-child(2) {
    opacity: 0;
}

.hamburger.active span:nth-child(3) {
    transform: translateY(-8.5px) rotate(-45deg);
}

.main-nav {
    margin-top: 32px;
    width: 100%;
    display: flex;
    justify-content: center;
    animation: fadeInDown 0.3s ease-out forwards;
}

.main-nav ul {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 20px;
    list-style: none;
    margin: 0;
    padding: 0;
}

.main-nav a {
    text-decoration: none;
    font-size: 22px;
    color: #1B3b5b;
    font-weight: 600;
    transition: color 0.2s ease;
}

.main-nav a:hover {
    color: #4a729e;
}

@keyframes fadeInDown {
    from {
        opacity: 0;
        transform: translateY(-10px);
    }

    to {
        opacity: 1;
        transform: translateY(0);
    }
}
</style>