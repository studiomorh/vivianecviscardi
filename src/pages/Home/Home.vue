<script setup>
import { ref } from 'vue'
import { AnimatePresence, motion } from 'motion-v'
import PackageBuilder from '../../components/PackageBuilder.vue'
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

const giftSizes = [1, 2, 3, 4, 5]
const bookingSizes = [1, 2, 3, 4]

const specificTechniques = [
  'Trattamento Personalizzato',
  ...treatments.map((treatment) => treatment.title),
]

const personalizedOpen = ref(false)
const specificOpen = ref(false)

const revealBox = {
  initial: { opacity: 0, height: 0 },
  animate: { opacity: 1, height: 'auto' },
  exit: { opacity: 0, height: 0 },
  transition: { duration: 0.45, ease: [0.22, 1, 0.36, 1] },
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

    <!-- Mobile floral background — user JPG, cover full width -->
    <div
      class="pointer-events-none fixed inset-0 bg-navy-deep bg-cover bg-center bg-no-repeat sm:hidden"
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

        <motion.div
          class="mt-12 flex w-full max-w-sm flex-col items-stretch gap-3 sm:max-w-none sm:flex-row sm:justify-center"
          :initial="{ opacity: 0, y: 10 }"
          :animate="{ opacity: 1, y: 0 }"
          :transition="{ delay: 0.7, duration: 0.75 }"
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
        <motion.div class="mx-auto max-w-md text-center" v-bind="fadeUp">
          <h2
            class="font-sans text-[1.15rem] font-semibold uppercase leading-snug tracking-[0.22em] text-white sm:text-[1.3rem] sm:tracking-[0.26em]"
          >
            Un trattamento su misura, oppure la tecnica che preferisci
          </h2>
          <p
            class="mt-8 font-sans text-[0.92rem] font-normal leading-[1.75] text-white/88"
          >
            Ogni corpo ha delle esigenze diverse, e per questo, il cuore del mio lavoro è
            un trattamento personalizzato: una seduta costruita dall’ascolto del corpo,
            nella quale posso integrare diverse tecniche manuali, ayurvediche,
            rilassanti, decontratturanti, drenanti e di stretching, in base alle esigenze
            di ogni persona.
          </p>
          <p
            class="mt-5 font-sans text-[0.92rem] font-normal leading-[1.75] text-white/78"
          >
            Ho sviluppato un approccio che integra tecniche e conoscenze diverse in modo
            attento e personalizzato. In ogni seduta, l’intensità, il ritmo e le tecniche
            utilizzate vengono scelti in base alle esigenze percepite nel corpo e
            all’obiettivo di ciascuna persona.
          </p>
          <p
            class="mt-5 font-sans text-[0.92rem] font-medium leading-[1.75] text-white"
          >
            Se sai già quale tecnica desideri ricevere, puoi anche scegliere direttamente
            uno dei trattamenti specifici.
          </p>
        </motion.div>

        <motion.article
          class="mx-auto max-w-md border border-white/70 px-6 py-10 text-center sm:px-8"
          v-bind="fadeUp"
        >
          <h2
            class="font-sans text-[1.15rem] font-semibold uppercase leading-snug tracking-[0.22em] text-white sm:text-[1.3rem] sm:tracking-[0.26em]"
          >
            Trattamento Personalizzato
          </h2>
          <p
            class="mt-8 font-sans text-[0.92rem] font-normal leading-[1.75] text-white/88"
          >
            Una seduta creata su misura, combinando tecniche diverse in base alle esigenze
            del corpo e al tuo obiettivo di benessere.
          </p>
          <p
            class="mt-5 font-sans text-[0.92rem] font-normal leading-[1.75] text-white/78"
          >
            È ideale per chi presenta diversi tipi di tensione, non sa esattamente quale
            trattamento scegliere oppure desidera un approccio più completo e
            personalizzato.
          </p>

          <button
            type="button"
            class="mt-10 inline-flex w-full items-center justify-center border border-white/70 px-6 py-3.5 font-sans text-[0.62rem] font-medium uppercase tracking-[0.28em] transition duration-300 sm:w-auto sm:px-8"
            :class="
              personalizedOpen
                ? 'bg-white text-navy-deep'
                : 'text-white hover:bg-white hover:text-navy-deep'
            "
            :aria-expanded="personalizedOpen"
            aria-controls="prenota-personalizzato"
            @click="personalizedOpen = !personalizedOpen"
          >
            Prenota trattamento personalizzato
          </button>

          <AnimatePresence>
            <motion.div
              v-if="personalizedOpen"
              id="prenota-personalizzato"
              class="overflow-hidden"
              v-bind="revealBox"
            >
              <div class="mt-8 border-t border-white/30 pt-8">
                <PackageBuilder
                  :whatsapp-base="whatsappBase"
                  :techniques="['Trattamento Personalizzato']"
                  :sizes="bookingSizes"
                  :default-size="1"
                  size-label="Quante sedute?"
                  unit-label="sedute"
                  total-label="Numero di sedute"
                  message-intro="Ciao Viviane! Vorrei prenotare un Trattamento Personalizzato."
                  message-outro="Puoi indicarmi le tue disponibilità? Grazie!"
                  submit-label="Prenota ora"
                  plain-message-label="Oppure invia un messaggio"
                />
              </div>
            </motion.div>
          </AnimatePresence>
        </motion.article>

        <motion.div class="mx-auto max-w-md text-center" v-bind="fadeUp">
          <h2
            class="font-sans text-[1.15rem] font-semibold uppercase leading-snug tracking-[0.22em] text-white sm:text-[1.3rem] sm:tracking-[0.26em]"
          >
            Massaggi specifici
          </h2>
          <p
            class="mt-6 font-sans text-[0.92rem] font-normal leading-[1.75] text-white/88"
          >
            Se preferisci un trattamento mirato, puoi scegliere tra:
          </p>
          <div class="mx-auto mt-10 h-px w-full bg-white/40" />
        </motion.div>

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

        <motion.div class="mx-auto max-w-md text-center" v-bind="fadeUp">
          <div class="mx-auto mb-12 h-px w-full bg-white/40" />
          <button
            type="button"
            class="inline-flex w-full items-center justify-center border border-white/70 px-6 py-3.5 font-sans text-[0.62rem] font-medium uppercase tracking-[0.28em] transition duration-300 sm:w-auto sm:px-8"
            :class="
              specificOpen
                ? 'bg-white text-navy-deep'
                : 'text-white hover:bg-white hover:text-navy-deep'
            "
            :aria-expanded="specificOpen"
            aria-controls="prenota-specifici"
            @click="specificOpen = !specificOpen"
          >
            Prenota un massaggio specifico
          </button>

          <AnimatePresence>
            <motion.div
              v-if="specificOpen"
              id="prenota-specifici"
              class="overflow-hidden"
              v-bind="revealBox"
            >
              <div class="mt-8 border border-white/40 px-5 py-8 sm:px-7">
                <PackageBuilder
                  :whatsapp-base="whatsappBase"
                  :techniques="specificTechniques"
                  :sizes="bookingSizes"
                  :default-size="1"
                  size-label="Quanti trattamenti?"
                  techniques-label="Scegli i trattamenti"
                  unit-label="trattamenti"
                  total-label="Totale trattamenti"
                  message-intro="Ciao Viviane! Vorrei prenotare dei trattamenti."
                  message-outro="Puoi indicarmi le tue disponibilità? Grazie!"
                  submit-label="Prenota ora"
                  plain-message-label="Oppure invia un messaggio"
                />
              </div>
            </motion.div>
          </AnimatePresence>
        </motion.div>
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

        <PackageBuilder
          class="mt-12"
          :whatsapp-base="whatsappBase"
          :techniques="giftTechniques"
          :sizes="giftSizes"
          :default-size="3"
          message-intro="Ciao Viviane! Vorrei comporre un buono regalo."
          message-outro="Puoi darmi più informazioni? Grazie!"
          submit-label="Monta il pacchetto"
        />
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
