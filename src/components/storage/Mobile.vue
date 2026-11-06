<script>
// Hulpfunctie en default data staan hier veilig in de module-scope
const getHeaderUrl = (folder, filename) => new URL(`../../assets/storage/${folder}/${filename}.jpg`, import.meta.url).href

const defaultStorageItems = [
    {
        id: 'motoren',
        title: 'Motoren',
        headerImgTop: getHeaderUrl('motorcycle', 'left'),
        headerImgBottom: getHeaderUrl('motorcycle', 'right'),
        features: [
            'Veilige en vorstvrije overdekte stalling',
            'Optie voor druppellader aansluiting',
            'Gemakkelijk en snel toegankelijk',
            'Vaste eigen plek voor uw motor'
        ]
    },
    {
        id: 'caravans',
        title: 'Caravans',
        headerImgTop: getHeaderUrl('caravan', 'left'),
        headerImgBottom: getHeaderUrl('caravan', 'right'),
        features: [
            'Ruime overdekte parkeerplaatsen',
            'Beschermd tegen alle weersomstandigheden',
            'Goed beveiligd terrein met cameratoezicht',
            'Geschikt voor zowel dagelijks als langdurig stallen'
        ]
    },
    {
        id: 'campers',
        title: 'Campers',
        headerImgTop: getHeaderUrl('camper', 'left'),
        headerImgBottom: getHeaderUrl('camper', 'right'),
        features: [
            'Extra ruime plekken met hoge doorrijhoogte',
            'Verhard en schoon terrein',
            'Handig voor de winterstalling of vakantie',
            'Ruim opstelterrein om gemakkelijk te manoeuvreren'
        ]
    }
]
</script>

<script setup>
import { ref, onMounted, onUnmounted, computed } from 'vue'

const props = defineProps({
    title: {
        type: String,
        default: 'Stalling voor uw voertuigen'
    },
    description: {
        type: String,
        default: 'Overdekte en beveiligde ruimte voor uiteenlopende voertuigen op Wibi Plein.'
    },
    items: {
        type: Array,
        default: () => defaultStorageItems
    }
})

const stackContainer = ref(null)
const scrollProgress = ref(0)
const scrollProgressPhase2 = ref(0)
const rawPhase3Scroll = ref(0)

const handleScroll = () => {
    if (!stackContainer.value) return
    const rect = stackContainer.value.getBoundingClientRect()
    const windowHeight = window.innerHeight
    const totalScrollable = stackContainer.value.offsetHeight - windowHeight
    if (totalScrollable <= 0) return

    const currentScroll = -rect.top
    let progress = currentScroll / totalScrollable
    scrollProgress.value = Math.max(0, Math.min(1, progress))

    const p2 = (currentScroll / (totalScrollable * 0.2))
    scrollProgressPhase2.value = Math.min(Math.max(p2, 0), 1)

    const phase3Scroll = Math.max(currentScroll - (totalScrollable * 0.2), 0)
    rawPhase3Scroll.value = phase3Scroll
}

onMounted(() => {
    window.addEventListener('scroll', handleScroll, { passive: true })
    handleScroll()
})

onUnmounted(() => {
    window.removeEventListener('scroll', handleScroll)
})

const currentActiveIndex = computed(() => {
    const scrollPerCard = 300
    const index = Math.floor(rawPhase3Scroll.value / scrollPerCard)
    return Math.min(Math.max(index, 0), props.items.length - 1)
})

const currentStorageType = computed(() => {
    return props.items[currentActiveIndex.value] || props.items[0]
})

const getCardTransform = (index) => {
    if (index === props.items.length - 1) {
        return 'translateX(0) scale(1)'
    }

    const stepSize = 1 / (props.items.length - 1)
    const cardStart = index * stepSize
    const cardEnd = (index + 1) * stepSize

    if (scrollProgress.value <= cardStart) {
        return 'translateX(0) scale(1)'
    } else if (scrollProgress.value >= cardEnd) {
        return 'translateX(110%) scale(0.95)'
    } else {
        const localProgress = (scrollProgress.value - cardStart) / stepSize
        const translateX = localProgress * 110
        const scale = 1 - (localProgress * 0.05)
        return `translateX(${translateX}%) scale(${scale})`
    }
}
</script>

<template>
    <section id="deposit">
        <div ref="stackContainer" class="scroll-runway">
            <div class="sticky-wrapper">
                <div class="widget-card">

                    <Transition name="fade">
                        <div v-if="scrollProgressPhase2 > 0.5" class="header-img header-img-top"
                            :style="{ backgroundImage: `url(${currentStorageType.headerImgTop})` }">
                        </div>
                    </Transition>

                    <div class="card-content-inner">
                        <div class="badge">Stalling</div>
                        <h2>{{ title }}</h2>
                        <p class="description">{{ description }}</p>

                        <div class="card-stack-area">
                            <div class="card-stack">
                                <div v-for="(item, index) in items" :key="index" class="info-box" :style="{
                                    transform: getCardTransform(index),
                                    zIndex: items.length - index
                                }">
                                    <h3>{{ item.title }}</h3>
                                    <ul v-if="item.features && item.features.length" class="feature-list">
                                        <li v-for="(feature, fIndex) in item.features" :key="fIndex">
                                            <span class="bullet">•</span>
                                            <span>{{ feature }}</span>
                                        </li>
                                    </ul>
                                </div>
                            </div>
                        </div>
                    </div>

                    <Transition name="fade">
                        <div v-if="scrollProgressPhase2 > 0.5" class="header-img header-img-bottom"
                            :style="{ backgroundImage: `url(${currentStorageType.headerImgBottom})` }">
                        </div>
                    </Transition>

                </div>
            </div>
        </div>
    </section>
</template>

<style scoped>
#deposit {
    max-width: 1000px;
    margin: 3rem auto;
    padding: 0 1.5rem;
}

.scroll-runway {
    position: relative;
    height: 300vh;
}

.sticky-wrapper {
    position: sticky;
    top: 100px;
}

.widget-card {
    position: relative;
    height: 1000px;
    max-width: 1000px;
    background-color: rgba(255, 255, 255, 0.85);
    border: 2px solid #1B3b5b;
    border-radius: 20px;
    padding: 2.5rem;
    box-shadow: 0 10px 30px rgba(27, 59, 91, 0.08);
    backdrop-filter: blur(10px);
    display: flex;
    flex-direction: column;
    box-sizing: border-box;
    overflow: hidden;
}

.header-img {
    position: absolute;
    top: 275px;
    left: 2.5rem;
    right: 2.5rem;
    height: 120px;
    border: 1px solid rgba(27, 59, 91, 0.2);
    border-radius: 12px;
    background-size: cover;
    background-position: center;
    background-repeat: no-repeat;
    z-index: 10;
    transition: background-image 0.4s ease-in-out;
}

.header-img-top {
    top: 800px;
}

.header-img-bottom {
    bottom: 2rem;
}

.card-content-inner {
    display: flex;
    flex-direction: column;
    height: 100%;
    justify-content: center;
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
    flex-shrink: 0;
    align-self: flex-start;
}

h2 {
    font-size: 2.25rem;
    color: #1B3b5b;
    font-weight: 700;
    margin-bottom: 1rem;
    line-height: 1.2;
    flex-shrink: 0;
}

.description {
    font-size: 1.1rem;
    color: #4a5568;
    line-height: 1.6;
    margin-bottom: 2rem;
    flex-shrink: 0;
}

.card-stack-area {
    position: relative;
    flex: 1;
    display: flex;
    align-items: flex-start;
}

.card-stack {
    width: 100%;
    margin-top: 200px;
    margin-bottom: 248px;
    display: grid;
    grid-template-areas: "stack";
    position: relative;
}

.info-box {
    grid-area: stack;
    background-color: #ffffff;
    border: 1px solid rgba(27, 59, 91, 0.15);
    border-radius: 16px;
    padding: 1.5rem 2rem;
    box-shadow: 0 12px 30px rgba(27, 59, 91, 0.1);
    will-change: transform;
    transition: transform 0.05s linear;
    box-sizing: border-box;
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

.fade-enter-active,
.fade-leave-active {
    transition: opacity 0.4s ease;
}

.fade-enter-from,
.fade-leave-to {
    opacity: 0;
}
</style>