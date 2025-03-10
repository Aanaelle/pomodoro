<template>
    <div id="app">
        <h1>Pomodoro</h1>
            <div class="container">
                <h2 id="pause">{{ title }}</h2>
                <div class="timer">
                    <button id="plus" class="button-timer button plus" @click="addMinute">
                    <img src="../images/plus.png" alt="plus" />
                    </button>
                    <div class="time">{{ formattedTime  }}</div>
                    <button id="moins" class="button-timer button" @click="subtractMinute">
                    <img src="../images/moins.png" alt="moins" />
                    </button>
                </div>

                <div class="action">
                    <button class="start button" @click="startTimer">
                    <img src="../images/start.png" alt="start" />
                    </button>
                    <button class="stop button" @click="stopTimer">
                    <img src="../images/stop.png" alt="stop" />
                    </button>
                    <button class="reset button" @click="resetTimer">
                    <img src="../images/reset.png" alt="reset" />
                    </button>
                </div>
            </div>
        </div>
</template>

<script setup>
import { ref, computed, onUnmounted } from 'vue';

const title = ref("Travail");
const minutes = ref(0);
const seconds = ref(3);
const isRunning = ref(false);
let intervalId = null;
const audio = new Audio('../sounds/lofi-295209.mp3')
const isPause = ref(false)

/**
 * Permet de formatter le minuteur
 */
const formattedTime = computed(() => {
  const m = String(minutes.value).padStart(2, '0');
  const s = String(seconds.value).padStart(2, '0');
  return `${m}:${s}`;
});


function startTimer() {
  if (isRunning.value) return;
  isRunning.value = true;
  title.value = "Travail";
  playMusic()
  intervalId = setInterval(() => {
    if (minutes.value === 0 && seconds.value === 0) {
      stopTimer();
      startPause();
      return;
    }
    if (seconds.value === 0) {
      minutes.value--;
      seconds.value = 59;
    } else {
      seconds.value--;
    }
  }, 1000);
}

function stopTimer() {
  isRunning.value = false;
  isPause.value = false
  clearInterval(intervalId);
}

function resetTimer() {
  stopTimer();
  minutes.value = 25;
  seconds.value = 0;
}
function addMinute() {
  minutes.value++;
}
function subtractMinute() {
  if (minutes.value > 0) {
    minutes.value--;
  }
}

function playMusic() {   
    audio.play();
}

function stopMusic() {
    audio.pause();
    audio.currentTime = 0;
}

function startPause() {
  if (isPause.value) return;
  isPause.value = true;
  title.value = "Pause";
  seconds.value = 5;
  intervalId = setInterval(() => {
    if (minutes.value === 0 && seconds.value === 0) {
      resetTimer();
      return;
    }
    if (seconds.value === 0) {
      minutes.value--;
      seconds.value = 59;
    } else {
      seconds.value--;
    }
  }, 1000);
}

onUnmounted(() => {
  stopTimer();
});

</script>

<style>
    body {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica,
        Arial, sans-serif;
    margin: auto;
    max-width: 38rem;
    padding: 2rem;
    background-color: #b7f3c8;
    }

    .timer {
    font-size: 3rem;
    font-weight: 700;
    text-align: center;
    }

    .button {
    background: none;
    border: none;
    }

    .button:active img {
    transform: translateY(2px);
    border-radius: 15px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3);
    }

    img {
    width: 200px;
    height: 100px;
    }

    .action {
    display: flex;
    }
</style>