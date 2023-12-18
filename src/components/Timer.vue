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
  <div class="computer-setup">
    <div class="computer">
      <div class="screen">
        <div class="timer-wrapper"
             :class="{'timer-wrapper--paused': isPaused,
       'timer-wrapper--finished': isFinished}"
        >
          <h5 class="title">
            Timer
          </h5>
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
      </div>
    </div>
    <div class="disk-drive">
      <div class="vents"></div>
    </div>
    <div class="mousepad">
      <div class="mouse" @click="toggleTimer"></div>
    </div>
  </div>
</template>



<style>
.computer-setup {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.computer {
  background-color: #c0c0c0;
  border: 4px solid #ababab;
  width: 500px;
  padding: 20px;
  z-index: 2;
}

.screen {
  background-color: #0000AA;
  border: 4px inset #ffffff;
  height: 300px;
  color: #ffffff;
  display: flex;
  justify-content: center;
  align-items: center;
}

.title {
  color: #ffffff;
  font-family: 'Courier New', monospace;
  font-size: 1.5em;
  text-align: center;
  margin-bottom: 10px;
}

.timer-wrapper {
}

.timer-wrapper {
  text-align: center;
}

.timer-wrapper--paused,
.timer-wrapper--finished {
}

.timer {
  display: flex;
  flex-direction: column;
  gap: 20px;
  font-size: 2em;
}

.timer__time {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: row;
  font-family: 'Courier New', monospace;
}

.timer__time__minutes,
.timer__time__seconds {
  font-size: 100px;
  font-family: "Courier New", Courier, monospace;
  color: #ffffff;
  line-height: 1;
}

.timer__time__colon {
  font-size: 50px;
  color: #ffffff;
  opacity: 0.7;
  line-height: 1;
}

.timer__controls {
  display: flex;
  justify-content: center;
  align-items: center;
}

.timer__controls__button {
  background-color: #000080;
  padding: 10px 20px;
  color: #ffffff;
  border: 2px outset #ffffff;
  font-size: 20px;
  font-weight: bold;
  margin: 0 10px;
  cursor: pointer;
  font-family: "Courier New", Courier, monospace;
  outline: none;
}

button.timer__controls__button:active {
  border-style: inset;
}

.timer__controls__button:hover {
  background-color: #000099;
}

.timer__controls__button:disabled {
  background-color: #808080;
  cursor: not-allowed;
}

.timer__controls__button:disabled:hover {
  background-color: #808080;
}

.disk-drive {
  background-color: #c0c0c0;
  width: 600px;
  height: 100px;
  border: 3px solid #ababab;
  border-radius: 4px;
  position: relative;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
  display: flex;
  align-items: center;
  justify-content: space-around;
  z-index: 1;
  margin-top: -2px;
}

.disk-drive::before {
  border: 2px inset #ffffff;
  content: '';
  position: absolute;
  top: 50%;
  left: 20px;
  width: 34px;
  height: 4px;
  background-color: #000000;
  transform: translateY(-50%);
  border-radius: 1px;
}

.disk-drive::after {
  content: '';
  position: absolute;
  top: 50%;
  right: 20px;
  width: 10px;
  height: 10px;
  background-color: #ff0000;
  border-radius: 50%;
  box-shadow: 0 0 8px #ff0000;
  transform: translateY(-50%);
}

.disk-drive .vents {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  right: 50px;
  height: 50%;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
}

.disk-drive .vents::before,
.disk-drive .vents::after {
  content: '';
  width: 100px;
  height: 2px;
  background-color: #000000;
  border: 2px inset #ffffff;
}

.mousepad {
  background-color: #000;
  width: 120px;
  height: 180px;
  margin-top: 10px;
  border-radius: 10px;
  position: relative;
}

.mouse {
  background-color: #c0c0c0;
  width: 40px;
  height: 60px;
  border-radius: 30px 30px 40px 40px;
  position: relative;
  top: 60px;
  left: 40px;
  box-shadow: 0 4px 0 rgba(255, 255, 255, 0.2);
}
.mouse::after {
  content: '';
  position: absolute;
  top: 5px;
  left: 50%;
  width: 10px;
  height: 20px;
  background-color: #808080;
  border-radius: 10px;
  transform: translateX(-50%);
  box-shadow: inset 0 2px 0 rgba(0, 0, 0, 0.1);
  cursor: pointer;
  border: 2px inset #ffffff;
}

</style>