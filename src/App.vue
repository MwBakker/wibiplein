<script setup>
import { ref, computed, onMounted, onUnmounted } from 'vue';
import NavBarDesktop from '@/components/base/nav/desktop.vue';
import NavBarMobile from '@/components/base/nav/mobile.vue';
import IntroDesktop from '@/components/intro/Desktop.vue';
import IntroMobile from '@/components/intro/Mobile.vue';
import StorageDesktop from '@/components/storage/Desktop.vue'; // Pas het pad eventueel aan naar jouw folderstructuur
import StorageMobile from '@/components/storage/Mobile.vue';   // Pas het pad eventueel aan naar jouw folderstructuur
import Deposits from '@/components/Deposits.vue';
import ContactForm from '@/components/ContactForm.vue';
import Footer from './components/base/Footer.vue';

const windowWidth = ref(typeof window !== 'undefined' ? window.innerWidth : 1200);

const handleResize = () => {
  windowWidth.value = window.innerWidth;
};

onMounted(() => {
  window.addEventListener('resize', handleResize);
});

onUnmounted(() => {
  window.removeEventListener('resize', handleResize);
});

const isMobile = computed(() => windowWidth.value < 768);

const storageWrapper = ref(null);
const isStorageVisible = ref(false);
let observer = null;

onMounted(() => {
  observer = new IntersectionObserver(
    ([entry]) => {
      if (entry.isIntersecting) {
        isStorageVisible.value = true;
        observer.disconnect();
      }
    },
    {
      threshold: 0.2
    }
  );

  if (storageWrapper.value) {
    observer.observe(storageWrapper.value);
  }
});

onUnmounted(() => {
  if (observer) observer.disconnect();
});
</script>

<template>
  <component :is="isMobile ? NavBarMobile : NavBarDesktop" />

  <div id="content">
    <component :is="isMobile ? IntroMobile : IntroDesktop" />

    <!-- Storage met Intersection Observer en dynamische component wissel -->
    <div ref="storageWrapper">
      <Transition name="fade-up">
        <component v-if="isStorageVisible" :is="isMobile ? StorageMobile : StorageDesktop" id="storage" />
      </Transition>
    </div>

    <Transition name="fade-up">
      <Deposits id="deposit" />
    </Transition>

    <ContactForm />
  </div>

  <Footer />
</template>

<style scoped>
#content {
  max-width: 1280px;
  margin: 0 auto;
  position: relative;
  padding-top: 176px;
}

#storage,
#deposit {
  margin-bottom: 720px;
}

.fade-up-enter-active {
  transition: all 0.8s ease-out;
}

.fade-up-enter-from {
  opacity: 0;
  transform: translateY(40px);
}

.fade-up-enter-to {
  opacity: 1;
  transform: translateY(0);
}

@media (max-width: 768px) {
  #content {
    padding-top: 116px;
  }
}
</style>