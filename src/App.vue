<template>
  <div class="timers">
    <TimerComponent
      v-for="timer in timers"
      :key="timer.id"
      :initialHours="timer.hours"
      :initialMinutes="timer.minutes"
      :initialSeconds="timer.seconds"
    ></TimerComponent>
    <button @click.prevent="addTimer" class="add-timer">
      <svg
        width="20"
        height="20"
        viewBox="0 0 20 20"
        fill="none"
        xmlns="http://www.w3.org/2000/svg"
      >
        <rect x="8.5" width="3" height="20" fill="white" />
        <rect
          y="11.5"
          width="3"
          height="20"
          transform="rotate(-90 0 11.5)"
          fill="white"
        />
      </svg>
    </button>
  </div>
</template>

<script setup>
import TimerComponent from "@/components/TimerComponent.vue";
import { ref } from "vue";

const timers = ref([
  { id: 1, hours: 1, minutes: 0, seconds: 0 },
  { id: 2, hours: 0, minutes: 45, seconds: 0 },
  { id: 3, hours: 0, minutes: 0, seconds: 15 },
]);

function addTimer() {
  const newTimer = {
    id: timers.value.length + 1,
    hours: 0,
    minutes: 0,
    seconds: 0,
  };

  timers.value.push(newTimer);
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

html,
body {
  width: 100%;
}

@font-face {
  font-family: "GothamPro";
  src: url("~@/assets/fonts/GothamPro.woff") format("woff");
  font-weight: 100;
  font-style: normal;
}

body {
  font-family: "GothamPro";
  font-size: 14px;
  background-color: #353638;
  color: white;
}

input,
button {
  font-family: "GothamPro";
}

input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}

#app {
  width: 100%;
  height: 100%;
}

.timers {
  display: grid;
  max-width: 775px;
  margin: 72px auto;
  justify-items: center;
  grid-template-columns: repeat(auto-fit, minmax(225px, 1fr));
  grid-gap: 45px;
}

.add-timer {
  cursor: pointer;
  width: 225px;
  height: 120px;
  display: flex;
  align-items: center;
  justify-content: center;
  border: none;
  outline: none;
  background-color: #696969;
}
</style>
