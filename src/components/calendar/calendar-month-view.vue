<template>
  <!--calendar month-view-->
  <div data-widget-item="calendar-inside">
    <!--calendar--header-->
    <div
      class="calendar--month-view--header grid grid-cols-repeat-7-minmax-0v1fr grid-flow-col w-full"
    >
      <!--day-column-cell-->
      <div
        class="select-none day-header w-full pt-1 px-2 text-left border-E0E0E0"
        v-for="(weekDayDate, weekindex) in weekDays"
        :class="{
          'border-r': weekindex !== weekDays.length - 1,
          'bg-FAFAFA': weekindex === weekDays.length - 1,
        }"
        :key="weekindex"
      >
        <!--dayname-->
        <span
          class="block text-71717A font-bold text-0dt625 leading-3 uppercase"
        >
          {{ dayName(weekDayDate, weekDayDate.getDate()).slice(0, -1) }}
        </span>
      </div>
    </div>
    <!--calendar--row-->
    <div
      class="calendar--month-view grid grid-cols-repeat-7-minmax-0v1fr grid-flow-col w-full"
      v-for="(line, index) in Math.ceil(monthDays.length / 7)"
      :key="index"
    >
      <!--day-row-cell-->
      <div
        class="relative select-none day-cell py-1 px-2 w-full min-h-5dt063 text-left border-b border-E0E0E0"
        v-for="(monthDayDate, monthdayindex) in Array.from(monthDays).slice(
          index * 7,
          line * 7
        )"
        :class="{
          'border-r': monthdayindex !== 6,
          'bg-FAFAFA': monthdayindex === 6,
        }"
        :key="monthdayindex"
      >
        <!-- events are here -->
        <span
          class="block text-black font-medium text-1dt375 leading-8"
          :class="{
            'calendar--month-view-not-in---month':
              monthDayDate.getMonth() !== dateSelected.getMonth(),
            'calendar--month-view-actual-day':
              monthDayDate.getDate() === dateSelected.getDate() &&
              monthDayDate.getMonth() === dateSelected.getMonth(),
          }"
        >
          {{ monthDayDate.getDate() }}
        </span>
        <!-- event component -->
        <Events
          class="relative mt-1"
          :eventDate="monthDayDate"
          :slots="slots"
        />
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import type { Slots } from "vue";

export interface Props {
  monthDays?: Date[];
  weekDays?: Date[];
  dateSelected: Date;
  slots: Slots;
}

import Events from "./calendar-event.vue";
import {
  twoDigitTime,
  incrementTime,
  fixDateTime,
  randomId,
  dayName,
  copyDate,
} from "./common";

const props = withDefaults(defineProps<Props>(), {
  monthDays: () => [],
  weekDays: () => [],
});
</script>

<style lang="scss" scoped>
.calendar--month-view-not-in---month {
  opacity: 0.5;
}

.calendar--month-view-actual-day {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  width: 28px;
  height: 28px;
  font-size: 16px;
  font-weight: bold;
  line-height: 18px;
  color: #fff;
  background: #0ea5e9;
  border-radius: 50%;
}
</style>
