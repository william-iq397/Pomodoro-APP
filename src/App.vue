<template>
  <div class="hero">
    <!-- TIMER SECTION -->
    <section class="pomodoro-timer">
      <h4 class="minutes"> {{ minutes < 10 ? '0' + minutes : minutes }}  </h4>
      <span>:</span>
      <h4 class="seconds"> {{ seconds < 10 ? '0' + seconds : seconds }}  </h4>
    </section>
    <!-- increment and decrement SECTION -->
    <section class="costume-time-container">
      <button class="plus" @click="increment">+</button>
      <button class="minus" @click="decrement">-</button>
    </section>
    <!-- start & stop section -->
    <section class="timer-start-end-container">
      <NewVue @click="startTimer()" v-if="isTimerStart" class="start-icon" />
      <Stop @click="stopTimer()" v-else class="stop-icon" />
      <Reset @click="resetTimer()" class="reset-icon" />
    </section>
  </div>
</template>

<script setup>
import { ref } from 'vue';
import Reset from './icons/reset.vue';
import NewVue from './icons/newVue.vue';
import Stop from './icons/Stop.vue';

const minutes = ref(50)
const seconds = ref(0)
let isTimerStart = ref(true);
let startWork;

// function to start the timer
function startTimer() {
  isTimerStart.value = false;
  startWork = setInterval(() => {
    // reused variables
    const isMinuteZero = minutes.value === 0;
    const isSecondZero = seconds.value === 0;

    // if minute ends => reset the seconds to 60 and decrement minute
    if (isSecondZero) {
      minutes.value--;
      seconds.value = 60;
    }

    // decrement seconds must be here!!
    seconds.value--

    // start break time
    if (isSecondZero && isMinuteZero) minutes.value = 9
  }, 1000);
}

// function to stop the timer
function stopTimer() {
  clearInterval(startWork)
  isTimerStart.value = true;
}

// increment the minutes
function increment() {
  minutes.value++
}

// decrement the minutes
function decrement() {
  if (minutes.value > 0) minutes.value--
}

// reset the timer
function resetTimer() {
  minutes.value = 50;
  seconds.value = 0;
}

</script> 

<style>
.hero {
  border-radius: 20px;
  width: 50%;
  height: 25rem;
  border: 1px solid white;
  display: flex;
  justify-content: space-evenly;
  align-items: center;
  flex-direction: column;
}

.pomodoro-timer {
  font-size: 2rem;
  width: 50%;
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 0.5em;
}

.costume-time-container {
  font-size: 2rem;
  width: 58%;
  display: flex;
  justify-content: space-evenly;
  padding: 0.5em;
}

.minutes,
.seconds {
  display: inline-block;
}

.plus,
.minus {
  cursor: pointer;
  width: 3rem;
  height: 3rem;
  font-size: 2.5rem;
  color: #000000;
  transition: all ease 0.2s;
  background-color: #ffffff;
  border-radius: 50%;
  border: 1px solid white;
  display: flex;
  justify-content: center;
  align-items: center;
}

.plus:hover, .minus:hover {
  color: #ffffff;
  background-color: #000000;
}

.timer-start-end-container {
  width: 60%;
  height: fit-content;
  display: flex;
  justify-content: space-evenly;
  align-items: center;
  padding: 0.5em;
}


.start-icon {
  width: 2rem;
  height: 2rem;
  cursor: pointer;
  scale: 1.2;
  transition: all ease 0.2s;
}

.stop-icon {
  width: 2rem;
  height: 2rem;
  cursor: pointer;
  transition: all ease 0.2s;
}

.reset-icon {
  width: 2em;
  height: 2em;
  cursor: pointer;
}

@media (max-width: 768px) {
  .hero {
    width: 98%;
    height: 98%;
  }
}
</style>