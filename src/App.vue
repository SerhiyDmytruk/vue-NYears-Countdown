<script setup>
import { ref, computed, onUnmounted } from 'vue';

import CountdownHeader from '@/components/CountdownHeader.vue';
import CountdownSegment from '@/components/CountdownSegment.vue';

const newYear = new Date(`01/01/${new Date().getFullYear() + 1} 00:00:00`);
const now = ref(new Date());

const interval = setInterval(() => (now.value = new Date()), 1000);
onUnmounted(() => clearInterval(interval));

const days = computed(() => {
  const one_day = 1000 * 60 * 60 * 24;
  return (newYear.getTime() - now.value.getTime()) / one_day;
});

const daysRounded = computed(() => Math.floor(days.value));

const hours = computed(() => 24 * (days.value - daysRounded.value));
const hoursRounded = computed(() => Math.floor(hours.value));

const mins = computed(() => 60 * (hours.value - hoursRounded.value));
const minsRounded = computed(() => Math.floor(60 * (hours.value - hoursRounded.value)));

const secs = computed(() => 60 * (mins.value - minsRounded.value));
const secsRounded = computed(() => Math.floor(secs.value));

</script>
<template>
  <div class="app-wrapper">
    <div class="countdown-box">
      <CountdownHeader />
      <main class="flex justify-center">
        <CountdownSegment data-test="days" label="days" :number="daysRounded" />
        <CountdownSegment data-test="hours" label="hours" :number="hoursRounded" />
        <CountdownSegment data-test="minutes" label="minutes" :number="minsRounded" />
        <CountdownSegment data-test="seconds" label="seconds" :number="secsRounded" />
      </main>
    </div>
  </div>
</template>

<style scoped>
.app-wrapper {
  @apply flex items-center justify-center w-full h-full p-10;
}
.countdown-box {
  @apply shadow-md relative bg-white p-5 rounded-lg border-gray-100 border-[1px];
}
body {
  @apply bg-gray-100;
}
</style>
