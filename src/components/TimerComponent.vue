<template>
  <div
    class="timer"
    :class="{ 'timer-finished': isFinished, 'timer-started': isRunning }"
  >
    <div class="timer--wrapper">
      <div v-if="isRunning" class="timer-time">
        {{ formattedTime }}
      </div>
      <div v-else class="timer-controls">
        <div class="timer-controls__input">
          <input
            type="number"
            id="hours"
            v-model.number="hours"
            :disabled="isRunning"
            @change="validateHours"
          />
        </div>
        <span>:</span>
        <div class="timer-controls__input">
          <input
            type="number"
            id="minutes"
            min="0"
            max="59"
            @change="validateMinutes"
            v-model.number="minutes"
            :disabled="isRunning"
          />
        </div>
        <span>:</span>
        <div class="timer-controls__input">
          <input
            type="number"
            id="seconds"
            min="0"
            max="59"
            @change="validateSeconds"
            v-model.number="seconds"
            :disabled="isRunning"
          />
        </div>
      </div>
    </div>
    <hr />
    <div class="timer-buttons">
      <button
        class="timer-buttons__button"
        v-if="!isRunning"
        @click.prevent="startTimer"
        :disabled="isRunning || !timeSet"
      >
        <svg
          width="17"
          height="20"
          viewBox="0 0 17 20"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <path d="M0 20V0L17 10L0 20Z" fill="white" />
        </svg>
      </button>
      <button
        class="timer-buttons__button"
        v-if="isRunning"
        @click.prevent="stopTimer"
        :disabled="!isRunning"
      >
        <svg
          width="10"
          height="20"
          viewBox="0 0 10 20"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <rect x="7" width="3" height="20" fill="white" />
          <rect width="3" height="20" fill="white" />
        </svg>
      </button>
      <button
        class="timer-buttons__button"
        @click.prevent="resetTimer"
        :disabled="!timeSet"
      >
        <svg
          width="20"
          height="20"
          viewBox="0 0 20 20"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
        >
          <rect width="20" height="20" fill="white" />
        </svg>
      </button>
    </div>
  </div>
</template>

<script>
import { computed, ref, toRefs } from "vue";

// Props
export default {
  props: {
    initialHours: {
      type: Number,
      default: 0,
    },
    initialMinutes: {
      type: Number,
      default: 0,
    },
    initialSeconds: {
      type: Number,
      default: 0,
    },
  },
  setup(props) {
    const { initialHours, initialMinutes, initialSeconds } = toRefs(props);

    const hours = ref(initialHours.value);
    const minutes = ref(initialMinutes.value);
    const seconds = ref(initialSeconds.value);
    const isRunning = ref(false);
    const isFinished = ref(false);
    const interval = ref(null);

    const timeSet = computed(
      () => hours.value > 0 || minutes.value > 0 || seconds.value > 0
    );
    const formattedTime = computed(() => {
      const fHours = hours.value.toString();
      const fMinutes = minutes.value.toString().padStart(2, "0");
      const fSeconds = seconds.value.toString().padStart(2, "0");
      return `${fHours}:${fMinutes}:${fSeconds}`;
    });

    function validateHours() {
      if (hours.value < 0) {
        return (hours.value = 0);
      }
    }
    function validateMinutes() {
      if (minutes.value > 59) {
        return (minutes.value = 59);
      }
      if (minutes.value < 0) {
        return (minutes.value = 0);
      }
    }
    function validateSeconds() {
      if (seconds.value > 59) {
        return (seconds.value = 59);
      }
      if (seconds.value < 0) {
        return (seconds.value = 0);
      }
    }
    function startTimer() {
      if (timeSet.value && !isRunning.value) {
        isRunning.value = true;
        isFinished.value = false;
        interval.value = setInterval(() => {
          if (seconds.value > 0) {
            seconds.value--;
          } else if (minutes.value > 0) {
            minutes.value--;
            seconds.value = 59;
          } else if (hours.value > 0) {
            hours.value--;
            minutes.value = 59;
            seconds.value = 59;
          } else {
            clearInterval(interval.value);
            isRunning.value = false;
            isFinished.value = true;
          }
        }, 1000);
      }
    }
    function stopTimer() {
      clearInterval(interval.value);
      isRunning.value = false;
    }
    function resetTimer() {
      hours.value = 0;
      minutes.value = 0;
      seconds.value = 0;
      clearInterval(interval.value);
      isRunning.value = false;
    }

    return {
      hours,
      minutes,
      seconds,
      isRunning,
      isFinished,
      formattedTime,
      timeSet,
      validateHours,
      validateMinutes,
      validateSeconds,
      startTimer,
      stopTimer,
      resetTimer,
    };
  },
};
// const props = defineProps({
// initialHours: {
//   type: Number,
//   default: 0,
// },
// initialMinutes: {
//   type: Number,
//   default: 0,
// },
// initialSeconds: {
//   type: Number,
//   default: 0,
// },
// });

// Variables
// const {
//   initialHours: hours,
//   initialMinutes: minutes,
//   initialSeconds: seconds,
// } = toRefs(props);

// const isRunning = ref(false);
// const isFinished = ref(false);
// const interval = ref(null);

// Computed
// const timeSet = computed(
//   () => hours.value > 0 || minutes.value > 0 || seconds.value > 0
// );
// const formattedTime = computed(() => {
//   const fHours = hours.value.toString();
//   const fMinutes = minutes.value.toString().padStart(2, "0");
//   const fSeconds = seconds.value.toString().padStart(2, "0");
//   return `${fHours}:${fMinutes}:${fSeconds}`;
// });

// Methods
// function validateHours() {
//   if (hours.value < 0) {
//     return (hours.value = 0);
//   }
// }
// function validateMinutes() {
//   if (minutes.value > 59) {
//     return (minutes.value = 59);
//   }
//   if (minutes.value < 0) {
//     return (minutes.value = 0);
//   }
// }
// function validateSeconds() {
//   if (seconds.value > 59) {
//     return (seconds.value = 59);
//   }
//   if (seconds.value < 0) {
//     return (seconds.value = 0);
//   }
// }
// function startTimer() {
//   if (timeSet.value && !isRunning.value) {
//     isRunning.value = true;
//     isFinished.value = false;
//     interval.value = setInterval(() => {
//       if (seconds.value > 0) {
//         seconds.value--;
//       } else if (minutes.value > 0) {
//         minutes.value--;
//         seconds.value = 59;
//       } else if (hours.value > 0) {
//         hours.value--;
//         minutes.value = 59;
//         seconds.value = 59;
//       } else {
//         clearInterval(interval);
//         isRunning.value = false;
//         isFinished.value = true;
//       }
//     }, 1000);
//   }
// }
// function stopTimer() {
//   clearInterval(interval);
//   isRunning.value = false;
// }
// function resetTimer() {
//   hours.value = 0;
//   minutes.value = 0;
//   seconds.value = 0;
//   clearInterval(interval);
//   isRunning.value = false;
// }
</script>

<style scoped>
hr {
  width: 100%;
}

span {
  font-size: 22px;
}

.timer {
  width: 225px;
  height: 120px;
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: #696969;
  opacity: 0.5;
}

.timer-started {
  opacity: 1;
}

.timer-finished {
  background-color: #445e43;
  border: 1px solid #4e6c4d;
  opacity: 1;
}

.timer--wrapper {
  width: 100%;
  height: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin: 0 auto;
}

.timer-time {
  font-size: 22px;
  color: white;
}

.timer-controls {
  display: flex;
  gap: 5px;
  max-width: 100%;
}

.timer-controls__input {
  max-width: 50px;
}

.timer-controls__input input {
  font-size: 22px;
  width: 100%;
  margin: 0;
  padding: 0;
  outline: 0;
  border: 0;
  background: none;
  border-bottom: 2px solid rgb(172, 172, 172);
  padding-bottom: 2px;
  color: white;
  text-align: center;
}

.timer-buttons {
  display: flex;
  align-items: center;
  justify-content: space-evenly;
  width: 100%;
  height: 50%;
}
.timer-buttons__button {
  cursor: pointer;
  border: none;
  outline: none;
  background: none;
}
</style>
