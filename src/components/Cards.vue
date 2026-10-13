<script setup>
import { ref, onMounted, onUnmounted, computed } from 'vue'
import Card from './small/Card.vue'

const isVisible = ref(false)
const sectionRef = ref(null)

const scrollProgressPhase1 = ref(0)
const scrollProgressPhase2 = ref(0)
const scrollProgressPhase3 = ref(0)
let observer = null

const getIconUrl = (name) => {
  return new URL(`../assets/icon/${name}.png`, import.meta.url).href
}

const getBgUrl = (name) => {
  return new URL(`../assets/bg/${name}.jpg`, import.meta.url).href
}

const stallingTypes = [
  {
    id: 'motoren',
    title: 'Motoren',
    icon: getIconUrl('motorcycle'),
    bgImage: getBgUrl('bg-motoren'),
    description: 'Veilige en droge plek voor de winterstop of het hele jaar door.',
    features: [
      'Mogelijkheid tot aansluiten druppellader',
      'Eenvoudig in- en uitrijden voor een rit',
      'Optimaal geventileerde ruimte'
    ]
  },
  {
    id: 'caravans',
    title: 'Caravans',
    icon: getIconUrl('caravan'),
    bgImage: getBgUrl('bg-caravans'),
    description: 'Ruime plekken met voldoende manoeuvreerruimte bij het halen en brengen.',
    features: [
      'Beschermd tegen UV-straling en vocht',
      'Schoon en droog voor het kampeerseizoen',
      'Brede doorgangen voor gemakkelijk parkeren'
    ]
  },
  {
    id: 'campers',
    title: 'Campers',
    icon: getIconUrl('camper'),
    bgImage: getBgUrl('bg-campers'),
    description: 'Geschikt voor zowel compacte buscampers als grote integraalcampers.',
    features: [
      'Royaal bemeten inrijhoogte',
      'Ruimte om op uw gemak in en uit te pakken',
      'Goede beveiliging van het terrein'
    ]
  },
  {
    id: 'oldtimers',
    title: 'Oldtimers',
    icon: getIconUrl('car'),
    bgImage: getBgUrl('bg-oldtimers'),
    description: 'Constante en geventileerde omgeving om uw klassieker in topconditie te houden.',
    features: [
      'Behoud van lak en metaal tegen roest',
      'Discreet en veilig ondergebracht',
      'Met liefde en zorg beheerd terrein'
    ]
  }
]

// Index van de huidige actieve kaart
const currentActiveIndex = computed(() => {
  return Math.min(
    Math.floor(scrollProgressPhase3.value),
    stallingTypes.length - 1
  )
})

const handleScroll = () => {
  if (!sectionRef.value) return

  const rect = sectionRef.value.getBoundingClientRect()
  const windowHeight = window.innerHeight

  const delayOffset = 850
  const currentScroll = windowHeight - rect.top - (windowHeight * 0.3) - delayOffset

  const distancePhase1 = 350
  const p1 = currentScroll / distancePhase1
  scrollProgressPhase1.value = Math.min(Math.max(p1, 0), 1)

  const distancePhase2 = 300
  const currentScrollPhase2 = currentScroll - distancePhase1
  const p2 = currentScrollPhase2 / distancePhase2
  scrollProgressPhase2.value = Math.min(Math.max(p2, 0), 1)

  const holdDistance = 400

  const distancePhase3 = 1000
  const currentScrollPhase3 = currentScrollPhase2 - distancePhase2 - holdDistance
  const p3 = (currentScrollPhase3 / distancePhase3) * (stallingTypes.length - 1)
  scrollProgressPhase3.value = Math.min(Math.max(p3, 0), stallingTypes.length - 1)
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll, { passive: true })

  observer = new IntersectionObserver(
    ([entry]) => {
      if (entry.isIntersecting) {
        isVisible.value = true
      }
    },
    { threshold: 0.05 }
  )

  if (sectionRef.value) {
    observer.observe(sectionRef.value)
  }
})

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll)
  if (observer) observer.disconnect()
})

const getCardTransform = (index) => {
  if (!isVisible.value) return ''

  const p1 = scrollProgressPhase1.value
  const p2 = scrollProgressPhase2.value
  const p3 = scrollProgressPhase3.value

  // Fase 1: In elkaar schuiven naar het centrum
  const maxOffset = 53.025
  const moveX = (index % 2 === 0 ? 1 : -1) * p1 * maxOffset
  const moveY = (index < 2 ? 1 : -1) * p1 * maxOffset

  const shiftLeft = p2 * -25

  let slideOutX = 0
  if (p2 === 1 && p3 > index) {
    const slideProgress = p3 - index
    slideOutX = Math.min(slideProgress, 1) * -100
  }

  return `translate(calc(${moveX}% + ${shiftLeft}vw + ${slideOutX}vw), ${moveY}%)`
}

const getCardStyle = (index) => {
  const p1 = scrollProgressPhase1.value
  const transform = getCardTransform(index)
  const delay = isVisible.value && p1 === 0 ? `${index * 0.12}s` : '0s'

  const bgOpacity = 0.75 + (p1 * 0.25)

  return {
    transitionDelay: delay,
    transform: transform,
    zIndex: stallingTypes.length - index,
    backgroundColor: `rgba(255, 255, 255, ${bgOpacity})`
  }
}
</script>

<template>
  <section ref="sectionRef" class="stalling-section">
    <div class="sticky-wrapper">
      <div class="bg-container">
        <div v-for="(item, index) in stallingTypes" :key="'bg-' + item.id" class="bg-layer" :style="{
          backgroundImage: `url(${item.bgImage})`,
          opacity: currentActiveIndex === index ? 1 : 0
        }" />
        <div class="bg-overlay"></div>
      </div>
      <div class="content-container">
        <div class="section-header">
          <h2>Onze Stalling & Mogelijkheden</h2>
          <p>Overdekte en beveiligde ruimte voor uiteenlopende voertuigen op Wibi Plein.</p>
        </div>
        <TransitionGroup name="fly-up" tag="div" class="cards-grid">
          <Card v-if="isVisible" v-for="(item, index) in stallingTypes" :key="item.id" :title="item.title"
            :description="item.description" :features="item.features" :icon-src="item.icon"
            :style="getCardStyle(index)" />
        </TransitionGroup>
      </div>

    </div>
  </section>
</template>

<style scoped>
.stalling-section {
  width: 100%;
  margin: 3rem 0;
  padding: 0;
  height: 400vh;
  position: relative;
}

.sticky-wrapper {
  position: sticky;
  top: 120px;
  transition: background-color 0.6s cubic-bezier(0.16, 1, 0.3, 1);
  padding: 2rem;
  border-radius: 24px;
}

.section-header {
  text-align: center;
  margin-bottom: 2.5rem;
}

.section-header h2 {
  font-size: 2rem;
  color: #1B3b5b;
  margin-bottom: 0.4rem;
  font-weight: 700;
}

.section-header p {
  font-size: 1rem;
  color: rgba(27, 59, 91, 0.8);
}

.cards-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 1.5rem;
  position: relative;
}

.fly-up-enter-from {
  opacity: 0;
  transform: translateY(60px);
}

.fly-up-enter-active {
  transition: opacity 0.8s cubic-bezier(0.16, 1, 0.3, 1), transform 0.8s cubic-bezier(0.16, 1, 0.3, 1);
}

.fly-up-enter-to {
  opacity: 1;
}

@media (max-width: 800px) {
  .cards-grid {
    grid-template-columns: 1fr;
  }
}
</style>