<script setup lang="ts">
  import {ref} from 'vue'

  let modalName = ref('Название окна');
  let modalText = ref("Lorem ipsum, dolor sit amet consectetur adipisicing elit. Molestiae beatae iure dolores corrupti exercitationem delectus eligendi veniam ipsum, reiciendis nesciunt?") 
  let header = ref('');
  let timeLimit = 30;

  let useCross = ref(false);
  function UseCross() {
    useCross.value = true;
  }

  let useTimer = ref(false);
  function UseTimer() {
    useTimer.value = true;
    startTimerModal();
  }

  function startTimerModal() {
    closeModal.value = '';
    hideTimer.value = '';
    timerInterval = setInterval(() => {
      timePassed = timePassed += 1;
      if (timeLeft > 0) {
        timeLeft = timeLimit - timePassed;
      } else if (timeLeft === 0) {
        closeModal.value = 'hidden';
        clearInterval(timerInterval);
      }
      if (timeLeft < 10) {
        modalTimerLabelPosition.value = "modal-timer__label-one-digit";
      }
      timerCounter.value = `${timeLeft % 60}`;
      circleDasharray.value = `${((timeLeft / timeLimit - (1 / timeLimit) * (1 - timeLeft / timeLimit)) * 283).toFixed(0)} 283`;
    }, 1000);
  }

  function removeCrossTimer() {
    useCross.value = false;
    useTimer.value = false;
  }

  let useConfirm = ref(false);
  function UseConfirm() {
    useConfirm.value = true;
  }

  let useIcon = ref(false);
  function UseIcon() {
    useIcon.value = true;
  }

  let confirmButtonText1 = ref('Yes');

  let useIconButton = ref(false);
  function UseIconButton() {
    useIconButton.value = true;
  }
  
  let useSecondButtonConfirm = ref(false);
  function UseSecondButtonConfirm() {
    useSecondButtonConfirm.value = true;
  }

  let confirmButtonText2 = ref('');

  let modalTimerLabelPosition = ref('');

  let timePassed = 0;
  let timeLeft = timeLimit;

  let timerInterval: any;
  let timerCounter = ref('');

  let circleDasharray = ref('');
  let remainingPathColor = ref('transparent');

  let closeModal = ref('');

  let hideTimer = ref('hidden')
  

</script>

<template>
  <div :class="`modal-window ${closeModal}`">
    <div class="modal-header">
      <p>{{ modalName }}</p>
      <div class="modal-header__close">
        <div v-if="useTimer" :class="`modal-timer ${hideTimer}`">
          <svg class="modal-timer__svg" viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
            <g class="modal-timer__circle">
              <circle class="modal-timer__path-elapsed" cx="50" cy="50" r="45" />
              <path
                :stroke-dasharray="circleDasharray"
                :class="`modal-timer__path-remaining ${remainingPathColor}`"
                d="M 50, 50 m -45, 0 a 45,45 0 1,0 90,0 a 45,45 0 1,0 -90,0"
              ></path>
            </g>
          </svg>
          <span :class="`modal-timer__label ${modalTimerLabelPosition}`">
            {{ timerCounter }}
          </span>
        </div>
        <div 
          v-if="useCross" 
          @click="closeModal = 'hidden'" 
          class="modal-cross"
        >
          <img src="/cross.svg" alt="Cancel">
        </div>
      </div>
    </div>
    <div class="modal__main">
      <img 
        v-if="useIcon" 
        src="/flag.svg" 
        alt="Flag"
      >
      <h3 v-if="header.length !== 0">{{ header }}</h3>
      <p 
        v-if="modalText.length !== 0" 
        :class="'align-text-center'"
      >
        {{ modalText }}
      </p>
    </div>
    <div v-if="useConfirm" class="modal__confirm">
      <button>
        <img 
          v-if="useIconButton" 
          src="/cross.svg" 
          alt="cross"
        >
        {{ confirmButtonText1 }}
      </button>
      <button v-if="useSecondButtonConfirm">{{ confirmButtonText2 }}</button>
    </div>
  </div>
</template>

<style scoped>

*{
  margin: 0;
  padding: 0;
  font-family: Arial, Helvetica, sans-serif;
}
.modal-window {
  width: 300px;
  height: fit-content;
}

.modal-header {
  padding: 3px;
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: #01143f;
  color: #589fe7;
  border-top-right-radius: 10px;
  border-top-left-radius: 10px;
}

.modal-header__close {
  position: absolute;
  padding-top: 5px;
  width: 20px;
  top: 0;
  right: 5px;
}

.modal__main {
  padding: 35px 20px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: #010b25;
  color: white;
  border-bottom: 3px solid #01143f;
  border-left: 3px solid #01143f;
  border-right: 3px solid #01143f;
}

.modal-timer__circle {
  fill: none;
  stroke: none;
}

.modal-timer__path-elapsed {
  stroke-width: 7px;
  stroke: #01143f;
}

.modal-timer__label {
  position: absolute;
  top: calc(50% - 8px);
  right: calc(50% - 6px);
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 10px;
}

.modal-timer__label-one-digit {
  right: calc(50% - 3px);
}

.modal-timer__path-remaining {
  stroke-width: 7px;
  stroke-linecap: round;
  transform: rotate(90deg);
  transform-origin: center;
  transition: 1s linear all;
  stroke: currentColor;
}

.modal__confirm {
  padding: 35px 20px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background-color: #010b25;
  color: white;
  border-bottom-left-radius: 10px;
  border-bottom-right-radius: 10px;
  border-bottom: 3px solid #01143f;
  border-left: 3px solid #01143f;
  border-right: 3px solid #01143f;
}

.modal__confirm button {
  padding: 5px;
}

.align-text-center {
  text-align: center;
}

.hidden {
  display: none;
}

</style>
