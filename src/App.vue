<template>
    <div id="app">
        <h1>Pomodoro</h1>
            <div class="container">
                <h2 id="title">{{ isPause ? "Pause" : "Travail" }}</h2>

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
import { ref, computed, onUnmounted, triggerRef } from 'vue';

const title = ref("Travail");
const minutes = ref(25);
const seconds = ref(0);
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

/**
 * Permet de lancer le miniteur de travail ou de pause
 */
function startTimer() {
  if (isRunning.value) return;
  isRunning.value = true;
  playMusic()
  intervalId = setInterval(() => {
    if (minutes.value === 0 && seconds.value === 0) {
      isPause.value = !isPause.value;
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

/**
 * Permet de mettre le minuteur sur pause
 */
function stopTimer() {
  isRunning.value = false;
  clearInterval(intervalId);
}

/**
 * Permet de remettre le minuteur à zéro
 */
function resetTimer() {
  stopTimer();

  minutes.value = isPause.value ? 5 : 25;
  seconds.value = 0;
}

/**
 * Permet d'ajouter des minutes au minuteur avec le bouton plus
 */
function addMinute() {
  minutes.value++;
}

/**
 * Permet de retirer des minutes au minuteur avec le bouton moins
 */
function subtractMinute() {
  if (minutes.value > 0) {
    minutes.value--;
  }
}

/**
 * Permet de lancer la musique 
 */
function playMusic() {   
    audio.play();
}

/**
 * Permet de mettre la musique sur pause
 */
function stopMusic() {
    audio.pause();
    audio.currentTime = 0;
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

    .title {
      margin-left: auto;
      margin-right: auto;
    }
</style>