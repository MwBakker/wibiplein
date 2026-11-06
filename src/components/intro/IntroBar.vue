<script setup>
import Card from '@/components/small/Card/Card.vue';

const getIconUrl = (name) => {
  return new URL(`../../assets/icon/${name}.png`, import.meta.url).href
}

const services = [
  {
    id: 'stalling',
    title: 'Stalling',
    icon: getIconUrl('warehouse'),
  },
  {
    id: 'statiegeld',
    title: 'Statiegeld',
    icon: getIconUrl('glass-bin'),
  }
]
</script>

<template>
  <section id="services">
    <div id="services-grid">
      <Transition name="fade-up" appear v-for="(service, index) in services" :key="service.id">
        <Card class="card-intro" :title="service.title" :icon-src="service.icon" :centered="true"
          :style="{ transitionDelay: `${index * 0.5}s` }" />
      </Transition>
    </div>
  </section>
</template>

<style scoped lang="scss">
@use '@/assets/transitions.scss';

#services {
  max-width: 900px;
  margin: 2rem auto;
  padding: 0 1rem;
}

.card-intro {
  width: 50%;
}

#services-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 1.5rem;
}

@media (max-width: 768px) {
  #services-grid {
    display: flex;
  }
}
</style>