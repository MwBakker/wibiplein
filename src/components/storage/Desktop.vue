<script setup>
import { ref, onMounted, onUnmounted, computed } from 'vue'
import Card from '../small/Card/Card.vue'
import CardContent from '../small/Card/Content.vue'

const isVisible = ref(false)
const sectionRef = ref(null)

const scrollProgressPhase1 = ref(0)
const scrollProgressPhase2 = ref(0)
const scrollProgressPhase3 = ref(0)
const rawPhase3Scroll = ref(0)
let observer = null

const getIconUrl = (name) => new URL(`../assets/icon/${name}.png`, import.meta.url).href
const getHeaderUrl = (folder, filename) => new URL(`../assets/storage/${folder}/${filename}.jpg`, import.meta.url).href

const storageTypes = [
  {
    id: 'motoren',
    title: 'Motoren',
    icon: getIconUrl('motorcycle'),
    headerImgTop: getHeaderUrl('motorcycle', 'left'),
    headerImgBottom: getHeaderUrl('motorcycle', 'right'),
    description: 'Veilige en droge plek voor de winterstop of het hele jaar door.',
    longDescription: 'Je motor is kwetsbaar voor vocht en temperatuurschommelingen. Vocht veroorzaakt roestvorming op de ketting en krammen, terwijl de accu langzaam leegloopt in de kou. Onze geventileerde en droge binnenruimte beschermt jouw tweewieler optimaal, met laadmogelijkheden zodat je in het voorjaar direct weer op pad kunt.',
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
    headerImgTop: getHeaderUrl('caravan', 'left'),
    headerImgBottom: getHeaderUrl('caravan', 'right'),
    description: 'Ruime plekken met voldoende manoeuvreerruimte bij het halen en brengen.',
    longDescription: 'Buitenstalling zorgt al snel voor vuil op het dak, groene aanslag en vochtschade aan naden en kitranden. Door je caravan overdekt en vorstvrij te parkeren blijft de carrosserie spik en span en verleng je de levensduur van de afdichtingen.',
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
    headerImgTop: getHeaderUrl('camper', 'left'),
    headerImgBottom: getHeaderUrl('camper', 'right'),
    description: 'Geschikt voor zowel compacte buscampers als grote integraalcampers.',
    features: [
      'Royaal bemeten inrijhoogte',
      'Ruimte om op uw gemak in en uit te pakken',
      'Goede beveiliging van het terrein'
    ],
    longDescription: 'Een camper is een kostbaar bezit dat veel te lijden heeft onder weersinvloeden. Onze ruime, vorstvrije stalling biedt een geconditioneerde omgeving waar je camper schoon, droog en direct klaar voor vertrek blijft.'
  },
  {
    id: 'oldtimers',
    title: 'Oldtimers',
    icon: getIconUrl('car'),
    headerImgTop: getHeaderUrl('oldtimer', 'left'),
    headerImgBottom: getHeaderUrl('oldtimer', 'right'),
    description: 'Constante en geventileerde omgeving om uw klassieker in topconditie te houden.',
    longDescription: 'Klassiekers vragen om speciale zorg en liefde. In onze afgesloten, constante omgeving blijft jouw waardevolle auto geventileerd en perfect geconditioneerd bewaard.',
    features: [
      'Behoud van lak en metaal tegen roest',
      'Discreet en veilig ondergebracht',
      'Met liefde en zorg beheerd terrein'
    ]
  }
]

const currentActiveIndex = computed(() => {
  const scrollPerCard = 400
  const index = Math.floor(rawPhase3Scroll.value / scrollPerCard)
  return Math.min(Math.max(index, 0), storageTypes.length - 1)
})

const currentStorageType = computed(() => {
  return storageTypes[currentActiveIndex.value] || storageTypes[0]
})

const handleScroll = () => {
  if (!sectionRef.value) return

  const rect = sectionRef.value.getBoundingClientRect()
  const windowHeight = window.innerHeight
  const currentScroll = windowHeight - rect.top - (windowHeight * 0.3) - 850

  const p1 = currentScroll / 350
  scrollProgressPhase1.value = Math.min(Math.max(p1, 0), 1)

  const currentScrollPhase2 = currentScroll - 350
  const p2 = currentScrollPhase2 / 300
  scrollProgressPhase2.value = Math.min(Math.max(p2, 0), 1)

  const currentScrollPhase3 = currentScrollPhase2 - 300 - 400
  rawPhase3Scroll.value = Math.max(currentScrollPhase3, 0)

  const distancePhase3 = 1000
  const p3 = (currentScrollPhase3 / distancePhase3) * (storageTypes.length - 1)
  scrollProgressPhase3.value = Math.min(Math.max(p3, 0), storageTypes.length - 1)
}

onMounted(() => {
  window.addEventListener('scroll', handleScroll, { passive: true })

  observer = new IntersectionObserver(
    ([entry]) => {
      if (entry.isIntersecting) isVisible.value = true
    },
    { threshold: 0.05 }
  )

  if (sectionRef.value) observer.observe(sectionRef.value)
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

  const moveX = (index % 2 === 0 ? 1 : -1) * p1 * 57.5
  const moveY = (index < 2 ? 1 : -1) * p1 * 57.5
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
  return {
    transitionDelay: isVisible.value && p1 === 0 ? `${index * 0.12}s` : '0s',
    transform: getCardTransform(index),
    zIndex: storageTypes.length - index,
    backgroundColor: `rgba(255, 255, 255, ${0.75 + (p1 * 0.25)})`
  }
}
</script>

<template>
  <section ref="sectionRef" id="storage">
    <div class="sticky-wrapper">
      <TransitionGroup name="fly-left">
        <div v-if="scrollProgressPhase2 === 1" class="header-img" id="header-img-top"
          :style="{ backgroundImage: `url(${currentStorageType.headerImgTop})` }">
        </div>
      </TransitionGroup>

      <div class="content-container">
        <div class="section-header">
          <h2>Onze Stalling &amp; Mogelijkheden</h2>
          <p>Overdekte en beveiligde ruimte voor uiteenlopende voertuigen op Wibi Plein.</p>
        </div>

        <TransitionGroup name="fly-up" tag="div" class="cards-grid">
          <Card v-if="isVisible" v-for="(item, index) in storageTypes" :key="item.id" :title="item.title"
            :description="item.description" :features="item.features" :icon-src="item.icon"
            :style="getCardStyle(index)" />
        </TransitionGroup>

        <Transition name="fade" mode="out-in">
          <CardContent v-if="isVisible && scrollProgressPhase2 > 0" :key="currentStorageType.id"
            :title="currentStorageType.title" :description="currentStorageType.longDescription" id="card-content" />
        </Transition>
      </div>

      <TransitionGroup name="fly-right">
        <div v-if="scrollProgressPhase2 === 1" class="header-img" id="header-img-bottom"
          :style="{ backgroundImage: `url(${currentStorageType.headerImgBottom})` }">
        </div>
      </TransitionGroup>
    </div>
  </section>
</template>

<style scoped lang="scss">
#storage {
  width: 100%;
  margin: 3rem 0;
  padding: 0;
  height: 4000px;
  position: relative;
  margin-bottom: 720px;

  #header-img-top {
    top: 0;
  }

  #header-img-bottom {
    bottom: -216px;
  }
}

.header-img {
  position: absolute;
  width: 100%;
  height: 320px;
  border: 2px solid #1B3b5b;
  border-radius: 20px;
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat;
  transition: background-image 0.4s ease-in-out;
}

#card-content {
  position: absolute;
  top: 460px;
  width: 50%;
  right: 12.5%;
}

.sticky-wrapper {
  position: sticky;
  top: 112px;
  transition: background-color 0.6s cubic-bezier(0.16, 1, 0.3, 1);
  padding: 2rem;
  border-radius: 24px;
}

.section-header {
  text-align: center;
  margin-bottom: 124px;

  h2 {
    font-size: 2rem;
    color: #1B3b5b;
    margin-bottom: 0.4rem;
    font-weight: 700;
  }

  p {
    font-size: 1rem;
    color: rgba(27, 59, 91, 0.8);
  }
}

.cards-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 48px;
}

.fly-left-enter-active,
.fly-left-leave-active,
.fly-right-enter-active,
.fly-right-leave-active {
  transition: all 0.6s cubic-bezier(0.16, 1, 0.3, 1);
}

.fly-left-enter-from,
.fly-left-leave-to {
  opacity: 0;
  transform: translateX(-50px);
}

.fly-right-enter-from,
.fly-right-leave-to {
  opacity: 0;
  transform: translateX(50px);
}

@media (max-width: 800px) {
  .cards-grid {
    grid-template-columns: 1fr;
  }
}
</style>