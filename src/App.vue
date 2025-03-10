<template>
  <h1>Pomodoro</h1>
    <div class="container">

      <div class="timer">
        <button id="plus" class="button-timer button plus">
          <img src="../images/plus.png" alt="plus" />
        </button>
        <div class="time">25:00</div>
        <button id="moins" class="button-timer button">
          <img src="../images/moins.png" alt="moins" />
        </button>
      </div>

      <div class="action">
        <button class="start button">
          <img src="../images/start.png" alt="start" />
        </button>
        <button class="stop button">
          <img src="../images/stop.png" alt="stop" />
        </button>
        <button class="reset button">
          <img src="../images/reset.png" alt="reset" />
        </button>
      </div>
    </div>
</template>

<script setup>
    console.log('👋 This message is being logged by "App.vue", included via Vite');
</script>

<style>
    body {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica,
        Arial, sans-serif;
    margin: auto;
    max-width: 38rem;
    padding: 2rem;
    background-color: #8bf3ff;
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