<script setup>
defineProps({
  title: {
    type: String,
    default: 'Statiegeld Inleveren op Wibi Plein'
  },
  description: {
    type: String,
    default: 'Snel, eenvoudig en zonder gedoe grote hoeveelheden blikjes en flessen inleveren. Onze opvangmachine verwerkt hele zakken tegelijk!'
  },
  features: {
    type: Array,
    default: () => [
      'Stort hele zakken met blikjes en plastic flessen in één keer',
      'Geen gedoe met stuk voor stuk invoeren',
      'Directe en snelle verwerking van uw statiegeld',
      'Ruim terrein en goed bereikbaar met de auto en aanhanger'
    ]
  }
})
</script>

<template>
  <section class="statiegeld-widget">
    <div class="widget-card">
      <div class="badge">Statiegeld Service</div>
      <h2>{{ title }}</h2>
      <p class="description">{{ description }}</p>

      <div class="info-box">
        <h3>Hoe werkt de automaat?</h3>
        <ul v-if="features.length" class="feature-list">
          <li v-for="(feature, index) in features" :key="index">
            <span class="bullet">•</span>
            <span>{{ feature }}</span>
          </li>
        </ul>
      </div>
    </div>
  </section>
</template>

<style scoped>
.statiegeld-widget {
  max-width: 1100px;
  margin: 3rem auto;
  padding: 0 1.5rem;
}

.widget-card {
  background-color: rgba(255, 255, 255, 0.85);
  border: 2px solid #1B3b5b;
  border-radius: 20px;
  padding: 2.5rem;
  box-shadow: 0 10px 30px rgba(27, 59, 91, 0.08);
  backdrop-filter: blur(10px);
}

.badge {
  display: inline-block;
  font-size: 0.8rem;
  text-transform: uppercase;
  letter-spacing: 0.05em;
  font-weight: 700;
  color: #2E8B57;
  background-color: rgba(46, 139, 87, 0.1);
  padding: 0.3rem 0.75rem;
  border-radius: 8px;
  margin-bottom: 1rem;
}

h2 {
  font-size: 2.25rem;
  color: #1B3b5b;
  font-weight: 700;
  margin-bottom: 1rem;
  line-height: 1.2;
}

.description {
  font-size: 1.1rem;
  color: #4a5568;
  line-height: 1.6;
  margin-bottom: 2rem;
}

.info-box {
  background-color: #ffffff;
  border: 1px solid rgba(27, 59, 91, 0.15);
  border-radius: 16px;
  padding: 1.5rem 2rem;
}

.info-box h3 {
  font-size: 1.3rem;
  color: #1B3b5b;
  font-weight: 700;
  margin-bottom: 1rem;
}

.feature-list {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
}

.feature-list li {
  font-size: 0.95rem;
  color: #2d3748;
  display: flex;
  align-items: flex-start;
  gap: 0.6rem;
  line-height: 1.5;
}

.bullet {
  color: #2E8B57;
  font-weight: bold;
  font-size: 1.2rem;
  line-height: 1;
}

@media (max-width: 768px) {
  .widget-card {
    padding: 1.5rem;
  }

  h2 {
    font-size: 1.75rem;
  }

  .info-box {
    padding: 1.25rem;
  }
}
</style>