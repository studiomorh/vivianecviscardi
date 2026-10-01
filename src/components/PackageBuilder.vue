<script setup>
import { computed, reactive, ref } from 'vue'

const props = defineProps({
  whatsappBase: { type: String, required: true },
  techniques: { type: Array, required: true },
  sizes: { type: Array, required: true },
  defaultSize: { type: Number, default: 1 },
  sizeLabel: { type: String, default: 'Quanti massaggi?' },
  techniquesLabel: { type: String, default: 'Scegli le tecniche' },
  unitLabel: { type: String, default: 'massaggi' },
  totalLabel: { type: String, default: 'Totale massaggi' },
  messageIntro: { type: String, required: true },
  messageOutro: { type: String, required: true },
  submitLabel: { type: String, required: true },
  plainMessageLabel: { type: String, default: '' },
})

const isSingle = computed(() => props.techniques.length === 1)

const packageSize = ref(props.defaultSize)
const counts = reactive(
  Object.fromEntries(props.techniques.map((technique) => [technique, 0])),
)

const allocated = computed(() =>
  isSingle.value
    ? packageSize.value
    : props.techniques.reduce((sum, technique) => sum + counts[technique], 0),
)
const remaining = computed(() => packageSize.value - allocated.value)
const isComplete = computed(
  () => packageSize.value >= 1 && allocated.value === packageSize.value,
)

const packageMessage = computed(() => {
  const lines = [props.messageIntro, '', `${props.totalLabel}: ${packageSize.value}`]

  if (!isSingle.value) {
    lines.push(
      '',
      ...props.techniques
        .filter((technique) => counts[technique] > 0)
        .map((technique) => `- ${technique}: ${counts[technique]}`),
    )
  }

  lines.push('', props.messageOutro)
  return lines.join('\n')
})

const packageWhatsappHref = computed(
  () => `${props.whatsappBase}?text=${encodeURIComponent(packageMessage.value)}`,
)

function clampCountsToSize(size) {
  if (isSingle.value) return

  let overflow = allocated.value - size
  for (let i = props.techniques.length - 1; i >= 0 && overflow > 0; i -= 1) {
    const technique = props.techniques[i]
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
</script>

<template>
  <div class="text-center">
    <div class="mx-auto max-w-md">
      <p class="font-sans text-[0.62rem] font-medium uppercase tracking-[0.32em] text-fog">
        {{ sizeLabel }}
      </p>
      <div class="mt-5 flex flex-wrap items-center justify-center gap-2.5">
        <button
          v-for="size in sizes"
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

    <div v-if="!isSingle" class="mx-auto mt-12 max-w-md text-left">
      <div class="mb-5 flex items-center justify-between gap-4">
        <p
          class="font-sans text-[0.62rem] font-medium uppercase tracking-[0.28em] text-fog"
        >
          {{ techniquesLabel }}
        </p>
        <p
          class="shrink-0 font-sans text-[0.62rem] font-medium uppercase tracking-[0.2em] text-white/75"
        >
          {{ allocated }} di {{ packageSize }} selezionati
        </p>
      </div>

      <ul class="space-y-1">
        <li
          v-for="technique in techniques"
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
      Distribuisci tutti i {{ packageSize }} {{ unitLabel }} per continuare.
    </p>

    <a
      v-if="isComplete"
      :href="packageWhatsappHref"
      target="_blank"
      rel="noopener noreferrer"
      class="mt-10 inline-flex min-w-[240px] items-center justify-center border border-white/70 bg-white/10 px-8 py-3.5 font-sans text-[0.65rem] font-medium uppercase tracking-[0.32em] text-white transition duration-300 hover:bg-white hover:text-navy-deep"
    >
      {{ submitLabel }}
    </a>
    <button
      v-else
      type="button"
      disabled
      class="mt-10 inline-flex min-w-[240px] cursor-not-allowed items-center justify-center border border-white/30 px-8 py-3.5 font-sans text-[0.65rem] font-medium uppercase tracking-[0.32em] text-white/40"
    >
      {{ submitLabel }}
    </button>

    <p v-if="plainMessageLabel" class="mt-6">
      <a
        :href="whatsappBase"
        target="_blank"
        rel="noopener noreferrer"
        class="font-sans text-[0.62rem] font-medium uppercase tracking-[0.28em] text-white/70 underline decoration-white/40 underline-offset-4 transition hover:text-white"
      >
        {{ plainMessageLabel }}
      </a>
    </p>
  </div>
</template>
