<template>
  <div v-if="results.length > 0" class="rounded-lg bg-white p-6 shadow-md">
    <RouteMap :results="results" />
    <h3 class="mt-6 mb-4 text-2xl font-medium text-gray-900">Route</h3>
    <div v-for="(result, index) in results" :key="index" class="bg-white">
      <div class="flex flex-col">
        <div class="grid grid-cols-[auto_1fr] grid-rows-[auto_2fr_auto] gap-x-3">
          <template v-if="index === 0 || results[index - 1].stayTime">
            <span class="row-start-1 self-center text-sm text-gray-600">
              {{ formatDateTime(result.startDateTime) }}
            </span>
            <div class="row-start-1 font-medium text-gray-900">{{ result.origin }}</div>
          </template>
          <span class="col-span-1 row-start-2 my-2 text-center text-gray-600">↓</span>
          <span class="col-span-1 row-start-2 my-2 self-center text-sm font-medium text-gray-900">
            {{ result.duration }}
          </span>
          <span class="row-start-3 self-center text-sm text-gray-600">
            {{ formatDateTime(result.arrivalDateTime) }}
          </span>
          <div class="row-start-3 font-medium text-gray-900">
            {{ result.destination }}
          </div>
        </div>

        <template v-if="result.stayTime">
          <div class="my-6 font-medium text-gray-900">
            Aufenthalt: {{ formatStayTime(result.stayTime) }}
          </div>
        </template>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import moment from 'moment'
import type { TransitResult } from '@/types/TransitResult'
import RouteMap from '@/components/RouteMap.vue'

export interface TransitResultProps {
  results: Array<TransitResult>
}

defineProps<TransitResultProps>()

// Not sure why this doesn't work with moment. Will maybe revisit it.
const formatStayTime = (msTime: string | null) => {
  if (!msTime) {
    return 'N/A'
  }

  const duration = moment.duration(parseInt(msTime, 10), 'seconds')
  const hours = Math.floor(duration.asHours()).toString().padStart(2, '0')
  const minutes = duration.minutes().toString().padStart(2, '0')
  return `${hours}:${minutes}`
}

const formatDateTime = (isoTime: string) => {
  return moment(isoTime).format('HH:mm')
}
</script>
