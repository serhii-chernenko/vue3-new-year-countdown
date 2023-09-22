<script setup>
import CountdownHeader from "@/components/CountdownHeader.vue";
import CountdownSegment from "@/components/CountdownSegment.vue";
import {reactive, watchEffect} from "vue";

const calculateRemainingTime = () => {
  const now = new Date();
  const newYear = new Date(now.getFullYear() + 1, 0, 1);

  return {
    days: Math.floor((newYear - now) / (1000 * 60 * 60 * 24)),
    hours: 23 - now.getHours(),
    minutes: 59 - now.getMinutes(),
    seconds: 59 - now.getSeconds(),
  }
};

const countdown = reactive(calculateRemainingTime());

watchEffect(() => {
  const interval = setInterval(() => Object.assign(countdown, calculateRemainingTime()), 1000);

  return () => clearInterval(interval);
})
</script>
<template>
  <div class="app-wrapper">
    <div class="countdown-box">
      <CountdownHeader />
      <main class="flex justify-center">
        <CountdownSegment data-test="days" label="days" :number="countdown.days" />
        <CountdownSegment data-test="hours" label="hours" :number="countdown.hours" />
        <CountdownSegment data-test="minutes" label="minutes" :number="countdown.minutes" />
        <CountdownSegment data-test="seconds" label="seconds" :number="countdown.seconds" />
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
