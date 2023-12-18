<script lang="ts">
import { defineComponent, ref, watch, onMounted } from 'vue';

export default defineComponent({
  name: 'Timer',
  components: {
  },
  setup() {
    const minutes = ref(0);
    const seconds = ref(0);
    const isPaused = ref(true);
    const isFinished = ref(false);
    const timer = ref<number | null>(null);

    const toggleTimer = () => {
      isPaused.value = !isPaused.value;
    };

    const resetTimer = () => {
      isPaused.value = true;
      isFinished.value = false;
      minutes.value = 0;
      seconds.value = 0;
    };

    const startTimer = () => {
      if (isFinished.value) {
        return;
      }

      if (seconds.value === 59) {
        seconds.value = 0;
        minutes.value++;
      } else {
        seconds.value++;
      }

      if (minutes.value === 25) {
        isFinished.value = true;
        isPaused.value = true;
      }
    };

    watch(isPaused, () => {
      if (!isPaused.value) {
        timer.value = setInterval(startTimer, 1000);
      } else {
        clearInterval(timer.value)
      }
    });

    onMounted(() => {
      resetTimer();
    });

    return {
      minutes,
      seconds,
      isPaused,
      isFinished,
      toggleTimer,
      resetTimer,
    };
  },
});
</script>
<template>
  <div class="timer-wrapper"
       :class="{'timer-wrapper--paused': isPaused,
       'timer-wrapper--finished': isFinished}"
  >
    <div class="timer">
      <div class="timer__time">
        <span class="timer__time__minutes">{{ minutes }}</span>
        <span class="timer__time__colon">:</span>
        <span class="timer__time__seconds">{{ seconds }}</span>
      </div>
      <div class="timer__controls">
        <button
          class="timer__controls__button"
          @click="toggleTimer"
          :disabled="isFinished"
        >
          {{ isPaused ? 'Start' : 'Pause' }}
        </button>
        <button
          class="timer__controls__button"
          @click="resetTimer"
          :disabled="isFinished"
        >
          Reset
        </button>
      </div>
    </div>
  </div>
</template>



<style>
@import url("https://fonts.googleapis.com/css2?family=Montserrat:wght@400;500;600&display=swap");



.timer-wrapper {
  background-color: var(--dark-blue-2);
  border-radius: 10px;
  padding: 20px;
  width: 400px;
  height: 400px;
  display: flex;
  justify-content: center;
  align-items: center;
  position: relative;
  transition: all 0.3s ease-in-out;
}

.timer-wrapper--paused {
  background-color: var(--dark-blue-3);
}

.timer-wrapper--finished {
  background-color: var(--dark-blue-4);
}

.timer {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}

.timer__time {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: row;
}

.timer__time__minutes,
.timer__time__seconds {
  font-size: 100px;
  font-weight: 600;
}

.timer__time__colon {
  font-size: 100px;
  font-weight: 600;
  opacity: 0.5;
}

.timer__controls {
  display: flex;
  justify-content: center;
  align-items: center;
}

.timer__controls__button {
  background-color: var(--dark-blue-5);
  padding: 10px 20px;
  color: #fff;
  border-radius: 10px;
  border: none;
  font-size: 20px;
  font-weight: 600;
  margin: 0 10px;
  cursor: pointer;
  transition: all 0.3s ease-in-out;
  outline: none;
}

// adjust border radius for all states if a button
button.timer__controls__button,
button.timer__controls__button:hover,
button.timer__controls__button:focus,
button.timer__controls__button:active {
  border-radius: 10px;
  border: none;
}

.timer__controls__button:hover {
  background-color: var(--dark-blue-6);
}

.timer__controls__button:disabled {
  background-color: var(--dark-blue-7);
  cursor: not-allowed;
}

.timer__controls__button:disabled:hover {
  background-color: var(--dark-blue-8);
}

</style>