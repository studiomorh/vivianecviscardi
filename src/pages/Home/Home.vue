<script setup>
import { computed, reactive, ref } from 'vue'
import { motion } from 'motion-v'
import bgMobile from '../../assets/images/bg-mobile.webp'

const whatsappBase = 'https://wa.me/393343101446'
const whatsapp = whatsappBase
const phoneDisplay = '334.3101446'
const phoneHref = 'tel:+393343101446'

const treatments = [
  {
    title: 'Massaggio Rilassante',
    duration: '60 minuti',
    intro:
      'Il Massaggio Rilassante è ideale per chi desidera ridurre lo stress e concedersi un momento di profondo benessere.',
    body:
      'Attraverso movimenti lenti, fluidi e avvolgenti favorisce il rilassamento muscolare, migliora la circolazione e aiuta a ritrovare calma ed equilibrio.',
    benefits: [
      'Riduce stress e tensioni.',
      'Favorisce il rilassamento psicofisico.',
      'Migliora la qualità del sonno.',
      'Dona una piacevole sensazione di leggerezza.',
    ],
  },
  {
    title: 'Massaggio Decontratturante',
    duration: '60 minuti',
    intro:
      'Pensato per alleviare tensioni e contratture muscolari localizzate, è particolarmente indicato per chi svolge lavori sedentari, pratica attività sportiva o soffre di rigidità muscolare.',
    body:
      'Il trattamento utilizza tecniche mirate per sciogliere le contratture e migliorare la mobilità.',
    benefits: [
      'Riduce il dolore muscolare.',
      'Scioglie le contratture.',
      'Migliora elasticità e mobilità.',
      'Favorisce il recupero muscolare.',
    ],
  },
  {
    title: 'Massaggio Yoga Ayurvedico',
    duration: '75 minuti',
    intro:
      'Un trattamento ispirato alla tradizione ayurvedica che combina tecniche di massaggio, stiramenti e movimenti dolci ispirati allo yoga.',
    body:
      'Favorisce il riequilibrio energetico, migliora la flessibilità e dona una profonda sensazione di armonia tra corpo e mente.',
    benefits: [
      'Migliora la mobilità articolare.',
      'Favorisce il rilassamento profondo.',
      "Stimola l'energia vitale.",
      'Aiuta a ritrovare equilibrio e benessere generale.',
    ],
  },
  {
    title: 'Massaggio Linfodrenante',
    duration: '60 minuti',
    intro:
      'Un trattamento delicato che stimola il sistema linfatico, favorendo il drenaggio dei liquidi e migliorando la circolazione linfatica.',
    body:
      'È particolarmente indicato in caso di gonfiore, ritenzione idrica e senso di pesantezza agli arti.',
    benefits: [
      'Riduce la ritenzione idrica.',
      'Favorisce il drenaggio dei liquidi.',
      'Dona leggerezza alle gambe.',
      'Migliora la circolazione linfatica.',
    ],
  },
]

const giftTechniques = [
  'Massaggio Decontratturante',
  'Massaggio Linfodrenante',
  'Massaggio Rilassante',
  'Massaggio Yoga Ayurvedico',
]

const packageSizes = [1, 2, 3, 4, 5]
const packageSize = ref(3)
const counts = reactive(
  Object.fromEntries(giftTechniques.map((technique) => [technique, 0])),
)

const allocated = computed(() =>
  giftTechniques.reduce((sum, technique) => sum + counts[technique], 0),
)
const remaining = computed(() => packageSize.value - allocated.value)
const isComplete = computed(
  () => packageSize.value >= 1 && allocated.value === packageSize.value,
)

const selectedLines = computed(() =>
  giftTechniques
    .filter((technique) => counts[technique] > 0)
    .map((technique) => `- ${technique}: ${counts[technique]}`),
)

const packageMessage = computed(() => {
  const lines = [
    'Ciao Viviane! Vorrei comporre un buono regalo.',
    '',
    `Totale massaggi: ${packageSize.value}`,
    '',
    ...selectedLines.value,
    '',
    'Puoi darmi più informazioni? Grazie!',
  ]
  return lines.join('\n')
})

const packageWhatsappHref = computed(
  () => `${whatsappBase}?text=${encodeURIComponent(packageMessage.value)}`,
)

function clampCountsToSize(size) {
  let overflow = allocated.value - size
  if (overflow <= 0) return

  for (let i = giftTechniques.length - 1; i >= 0 && overflow > 0; i -= 1) {
    const technique = giftTechniques[i]
    const removable = Math.min(counts[technique], overflow)
    counts[technique] -= removable
    overflow -= removable
  }
}

function setPackageSize(size) {
  packageSize.value = size
  clampCountsToSize(size)
}

function inc(technique) {
  if (remaining.value <= 0) return
  counts[technique] += 1
}

function dec(technique) {
  if (counts[technique] <= 0) return
  counts[technique] -= 1
}

const fadeUp = {
  initial: { opacity: 0, y: 28 },
  whileInView: { opacity: 1, y: 0 },
  viewport: { once: true, amount: 0.25 },
  transition: { duration: 0.7, ease: [0.22, 1, 0.36, 1] },
}
</script>

<template>
  <div class="relative min-h-screen overflow-x-hidden bg-navy-deep text-mist">
    <!-- Desktop / tablet fallback atmosphere -->
    <div
      class="pointer-events-none absolute inset-0 hidden bg-[radial-gradient(ellipse_at_top,rgba(255,255,255,0.06),transparent_55%),linear-gradient(180deg,#0a1630_0%,#050b18_45%,#07101f_100%)] sm:block"
    />
    <img
      src="../../assets/images/organic-lines.svg"
      alt=""
      class="pointer-events-none absolute inset-0 hidden h-full w-full object-cover sm:block"
    />

    <!-- Mobile floral background from brand PDF (high-res) -->
    <div
      class="pointer-events-none absolute inset-0 bg-navy-deep bg-repeat-y bg-[length:100%_auto] sm:hidden"
      :style="{ backgroundImage: `url(${bgMobile})` }"
      aria-hidden="true"
    />

    <div class="relative z-10 mx-auto w-full max-w-[34rem] px-6 pb-24 sm:px-8">
      <!-- Hero -->
      <motion.header
        class="flex min-h-[100svh] flex-col items-center justify-center py-20 text-center"
        :initial="{ opacity: 0 }"
        :animate="{ opacity: 1 }"
        :transition="{ duration: 1.1, ease: 'easeOut' }"
      >
        <motion.h1
          class="font-sans text-[1.85rem] font-semibold uppercase leading-[1.15] tracking-[0.28em] text-white sm:text-[2.35rem] sm:tracking-[0.32em]"
          :initial="{ opacity: 0, y: 18 }"
          :animate="{ opacity: 1, y: 0 }"
          :transition="{ delay: 0.2, duration: 0.9 }"
        >
          Viviane C.
          <span class="mt-3 block tracking-[0.36em]">Viscardi</span>
        </motion.h1>

        <motion.p
          class="mt-7 font-sans text-[0.68rem] font-medium uppercase tracking-[0.42em] text-white/85"
          :initial="{ opacity: 0 }"
          :animate="{ opacity: 1 }"
          :transition="{ delay: 0.45, duration: 0.8 }"
        >
          Massoterapeuta
        </motion.p>

        <motion.p
          class="mt-5 font-sans text-[0.65rem] font-light uppercase tracking-[0.5em] text-fog"
          :initial="{ opacity: 0, y: 10 }"
          :animate="{ opacity: 1, y: 0 }"
          :transition="{ delay: 0.55, duration: 0.8 }"
        >
          2026
        </motion.p>

        <motion.p
          class="mt-12 border border-white/70 px-5 py-3.5 font-sans text-[0.62rem] font-medium uppercase tracking-[0.28em] text-white sm:tracking-[0.34em]"
          :initial="{ opacity: 0, y: 10 }"
          :animate="{ opacity: 1, y: 0 }"
          :transition="{ delay: 0.7, duration: 0.75 }"
        >
          Massaggio · I Trattamenti · Percorso
        </motion.p>

        <motion.div
          class="mt-12 flex w-full max-w-sm flex-col items-stretch gap-3 sm:max-w-none sm:flex-row sm:justify-center"
          :initial="{ opacity: 0, y: 10 }"
          :animate="{ opacity: 1, y: 0 }"
          :transition="{ delay: 0.85, duration: 0.75 }"
        >
          <a
            href="#regalo"
            class="inline-flex min-h-12 items-center justify-center border border-white/70 px-8 py-3.5 font-sans text-[0.65rem] font-medium uppercase tracking-[0.32em] text-white transition duration-300 hover:bg-white hover:text-navy-deep"
          >
            Buono regalo
          </a>
          <a
            href="#trattamenti"
            class="inline-flex min-h-12 items-center justify-center border border-white/70 px-8 py-3.5 font-sans text-[0.65rem] font-medium uppercase tracking-[0.32em] text-white transition duration-300 hover:bg-white/10"
          >
            I trattamenti
          </a>
        </motion.div>
      </motion.header>

      <!-- Treatments -->
      <section id="trattamenti" class="space-y-28 py-8 sm:space-y-32">
        <motion.article
          v-for="(item, index) in treatments"
          :key="item.title"
          class="mx-auto max-w-md text-center"
          v-bind="fadeUp"
          :transition="{ ...fadeUp.transition, delay: index * 0.05 }"
        >
          <h2
            class="font-sans text-[1.15rem] font-semibold uppercase leading-snug tracking-[0.22em] text-white sm:text-[1.3rem] sm:tracking-[0.26em]"
          >
            {{ item.title }}
          </h2>
          <p
            class="mt-5 font-sans text-[0.62rem] font-semibold uppercase tracking-[0.28em] text-white/90"
          >
            Durata: {{ item.duration }}
          </p>
          <p
            class="mt-8 font-sans text-[0.92rem] font-normal leading-[1.75] text-white/88"
          >
            {{ item.intro }}
          </p>
          <p
            class="mt-5 font-sans text-[0.92rem] font-normal leading-[1.75] text-white/78"
          >
            {{ item.body }}
          </p>
          <ul class="mt-9 space-y-3">
            <li
              v-for="benefit in item.benefits"
              :key="benefit"
              class="font-sans text-[0.9rem] font-medium leading-snug text-white"
            >
              {{ benefit }}
            </li>
          </ul>
        </motion.article>
      </section>

      <!-- Mid CTA -->
      <motion.section
        class="flex flex-col items-center py-28 text-center"
        v-bind="fadeUp"
      >
        <a
          :href="whatsapp"
          target="_blank"
          rel="noopener noreferrer"
          class="inline-flex min-w-[240px] items-center justify-center border border-white/70 px-10 py-4 font-sans text-[0.65rem] font-medium uppercase tracking-[0.32em] text-white transition duration-300 hover:bg-white hover:text-navy-deep"
        >
          Invia messaggio
        </a>
        <p
          class="mt-8 max-w-sm font-sans text-[0.92rem] font-normal leading-[1.7] text-white/80"
        >
          Ti aspetto per accompagnarti in un percorso dedicato al tuo benessere.
        </p>
      </motion.section>

      <!-- Gift voucher builder -->
      <motion.section id="regalo" class="pb-10 pt-4 text-center" v-bind="fadeUp">
        <h2
          class="font-sans text-[1.7rem] font-semibold uppercase leading-[1.2] tracking-[0.28em] text-white sm:text-[2rem] sm:tracking-[0.32em]"
        >
          Regala
          <span class="mt-2 block tracking-[0.28em]">Benessere</span>
        </h2>

        <div
          class="mx-auto mt-10 inline-flex bg-white px-8 py-2.5 font-sans text-[0.62rem] font-semibold uppercase tracking-[0.35em] text-navy-deep"
        >
          Buono regalo
        </div>

        <p
          class="mx-auto mt-10 max-w-md font-sans text-[0.95rem] font-normal leading-[1.7] text-white/90"
        >
          Da 1 a 5 massaggi, da scegliere per te o da regalare!
        </p>
        <p
          class="mx-auto mt-5 max-w-md font-sans text-[0.9rem] font-normal leading-[1.7] text-white/75"
        >
          Componi il tuo buono scegliendo liberamente tra le seguenti tecniche:
        </p>

        <div class="mx-auto mt-12 max-w-md">
          <p
            class="font-sans text-[0.62rem] font-medium uppercase tracking-[0.32em] text-fog"
          >
            Quanti massaggi?
          </p>
          <div class="mt-5 flex flex-wrap items-center justify-center gap-2.5">
            <button
              v-for="size in packageSizes"
              :key="size"
              type="button"
              class="inline-flex h-11 w-11 items-center justify-center border font-sans text-sm font-medium transition duration-300"
              :class="
                packageSize === size
                  ? 'border-white bg-white text-navy-deep'
                  : 'border-white/70 text-mist hover:border-white hover:text-white'
              "
              :aria-pressed="packageSize === size"
              @click="setPackageSize(size)"
            >
              {{ size }}
            </button>
          </div>
        </div>

        <div class="mx-auto mt-12 max-w-md text-left">
          <div class="mb-5 flex items-center justify-between gap-4">
            <p
              class="font-sans text-[0.62rem] font-medium uppercase tracking-[0.28em] text-fog"
            >
              Scegli le tecniche
            </p>
            <p
              class="font-sans text-[0.62rem] font-medium uppercase tracking-[0.2em] text-white/75"
            >
              {{ allocated }} di {{ packageSize }} selezionati
            </p>
          </div>

          <ul class="space-y-1">
            <li
              v-for="technique in giftTechniques"
              :key="technique"
              class="flex items-center justify-between gap-4 border-b border-white/20 py-3.5"
            >
              <span
                class="font-sans text-[0.78rem] font-medium uppercase tracking-[0.1em] text-white/90"
              >
                {{ technique }}
              </span>
              <div class="flex shrink-0 items-center gap-3">
                <button
                  type="button"
                  class="inline-flex h-8 w-8 items-center justify-center border border-white/70 text-mist transition hover:border-white hover:text-white disabled:cursor-not-allowed disabled:opacity-30"
                  :disabled="counts[technique] <= 0"
                  :aria-label="`Riduci ${technique}`"
                  @click="dec(technique)"
                >
                  −
                </button>
                <span
                  class="w-4 text-center font-sans text-sm font-medium tabular-nums text-white"
                >
                  {{ counts[technique] }}
                </span>
                <button
                  type="button"
                  class="inline-flex h-8 w-8 items-center justify-center border border-white/70 text-mist transition hover:border-white hover:text-white disabled:cursor-not-allowed disabled:opacity-30"
                  :disabled="remaining <= 0"
                  :aria-label="`Aumenta ${technique}`"
                  @click="inc(technique)"
                >
                  +
                </button>
              </div>
            </li>
          </ul>
        </div>

        <p
          v-if="!isComplete"
          class="mx-auto mt-8 max-w-md font-sans text-[0.85rem] font-normal text-white/65"
        >
          Distribuisci tutti i {{ packageSize }} massaggi per continuare.
        </p>

        <a
          v-if="isComplete"
          :href="packageWhatsappHref"
          target="_blank"
          rel="noopener noreferrer"
          class="mt-10 inline-flex min-w-[240px] items-center justify-center border border-white/70 bg-white/10 px-8 py-3.5 font-sans text-[0.65rem] font-medium uppercase tracking-[0.32em] text-white transition duration-300 hover:bg-white hover:text-navy-deep"
        >
          Monta il pacchetto
        </a>
        <button
          v-else
          type="button"
          disabled
          class="mt-10 inline-flex min-w-[240px] cursor-not-allowed items-center justify-center border border-white/30 px-8 py-3.5 font-sans text-[0.65rem] font-medium uppercase tracking-[0.32em] text-white/40"
        >
          Monta il pacchetto
        </button>
      </motion.section>

      <!-- Contacts -->
      <motion.footer
        id="contatti"
        class="mt-24 border-t border-white/25 pt-16 text-center"
        v-bind="fadeUp"
      >
        <p
          class="font-sans text-[0.65rem] font-medium uppercase tracking-[0.42em] text-white"
        >
          Contatti
        </p>
        <a
          :href="phoneHref"
          class="mt-8 inline-block font-sans text-base font-medium tracking-[0.14em] text-white transition hover:opacity-80"
        >
          {{ phoneDisplay }}
        </a>
        <a
          :href="whatsapp"
          target="_blank"
          rel="noopener noreferrer"
          class="mt-10 inline-flex items-center justify-center border border-white/70 px-8 py-3 font-sans text-[0.62rem] font-medium uppercase tracking-[0.32em] text-white transition duration-300 hover:bg-white hover:text-navy-deep"
        >
          Scrivimi su WhatsApp
        </a>
      </motion.footer>
    </div>
  </div>
</template>
