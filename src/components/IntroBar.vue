<script setup>
import Card from '@/components/small/Card.vue'

const getIconUrl = (name) => {
  return new URL(`../assets/icon/${name}.png`, import.meta.url).href
}

const services = [
  {
    id: 'stalling',
    title: 'Stalling',
    icon: getIconUrl('caravan'),
    description: 'Veilige, droge en overdekte stalling voor caravans, campers en klassiekers.',
    features: [
      'Beveiligd terrein',
      'Goede ventilatie',
      'Flexibel ophalen en brengen'
    ]
  },
  {
    id: 'statiegeld',
    title: 'Statiegeld',
    icon: getIconUrl('recycle'),
    description: 'Snel en milieubewust grote hoeveelheden statiegeldflessen en -blikjes inleveren.',
    features: [
      'Zakken in één keer storten',
      'Geen stuk-voor-stuk gedoe',
      'Directe verwerking'
    ]
  }
]
</script>

<template>
  <section class="services-section">
    <div class="services-grid">
      <StallingCard
        v-for="service in services"
        :key="service.id"
        :title="service.title"
        :description="service.description"
        :features="service.features"
        :icon-src="service.icon"
      />
    </div>
  </section>
</template>

<style scoped lang="scss">
.services-section {
  max-width: 900px;
  margin: 2rem auto;
  padding: 0 1rem;
}

.services-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 1.5rem;

  @media (max-width: 650px) {
    grid-template-columns: 1fr;
  }
}
</style>