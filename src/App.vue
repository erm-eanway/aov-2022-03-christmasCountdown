<script setup lang="ts">
import CountdownHeader from '@/components/CountdownHeader.vue'
import CountdownSegment from './components/CountdownSegment.vue'
import { useNow } from '@vueuse/core'
import { computed } from 'vue'

const now = useNow()
const christmas = new Date('12/25/2022 00:00:00')

function Countdown(start, end) {
  const hours_in_day = 24
  const minutes_in_hour = 60
  const seconds_in_minute = 60
  const milliseconds_in_second = 1000

  const milliseconds_in_day = hours_in_day * minutes_in_hour * seconds_in_minute * milliseconds_in_second

  const decimalDays = computed(() => {
    return (end.getTime() - start.value.getTime()) / milliseconds_in_day
  })

  const decimalHours = computed(() => {
    return hours_in_day * (decimalDays.value - this.days.value)
  })

  const decimalMinutes = computed(() => {
    return minutes_in_hour * (decimalHours.value - this.hours.value)
  })

  const decimalSeconds = computed(() => {
    return seconds_in_minute * (decimalMinutes.value - this.minutes.value)
  })

  // getters
  this.days = computed(() => {
    return Math.floor(decimalDays.value)
  })

  this.hours = computed(() => {
    return Math.floor(decimalHours.value)
  })

  this.minutes = computed(() => {
    return Math.floor(decimalMinutes.value)
  })

  this.seconds = computed(() => {
    return Math.floor(decimalSeconds.value)
  })
}

const christmasCountdown = new Countdown(now, christmas)
</script>

<template>
  <div class="w-full h-full flex justify-center items-center p-10">
    <div>
      <div class="shadow-md relative bg-white p-5 rounded-lg border-gray-100 border-[1px]">
        <CountdownHeader />
        <main class="flex justify-center">
          <CountdownSegment label="days" :number="christmasCountdown.days.value" />
          <CountdownSegment label="hours" :number="christmasCountdown.hours.value" />
          <CountdownSegment label="minutes" :number="christmasCountdown.minutes.value" />
          <CountdownSegment label="seconds" :number="christmasCountdown.seconds.value" />
        </main>
      </div>
      <h4 class="mt-10 text-gray-400 text-center text-sm">
        This challenge brought to you by <a href="https://vueschool.io/" class="underline">Vue School</a>
      </h4>
    </div>
  </div>
</template>

<style>
div {
  display: block;
}

body {
  @apply bg-gray-100;
}
</style>
