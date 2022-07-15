<template>
  <div class="container">
    <div id="timerContainer">
      <div>
        <div v-if="displayWorkTime > 0">
          <h1>Work</h1>
          <div class="timer">
            {{ formatTime(this.displayWorkTime) }}
          </div>
        </div>
        <div v-else>
          <h1>Break</h1>
          <div class="timer">
            {{ formatTime(this.displayBreakTime) }}
          </div>
        </div>
      </div>
      <div class="row">
        <button id="start-stop" class="button" @click="startStop()">
          {{ this.timerRunning ? "Stop" : "Start" }}
        </button>
        <button id="reset" class="button" @click="reset()">Reset</button>
      </div>
      <div class="row">
        <div>
          <button
            @click="
              setTime('work', 50);
              setTime('break', 10);
            "
            class="button"
          >
            50 / 10
          </button>
        </div>
        <div>
          <button
            @click="
              setTime('work', 45);
              setTime('break', 15);
            "
            class="button"
          >
            45 / 15
          </button>
        </div>
        <div>
          <button
            @click="
              setTime('work', 25);
              setTime('break', 5);
            "
            class="button"
          >
            25 / 5
          </button>
        </div>
      </div>
      <!-- <form>
        <div>
          Work Timer:
          <input
            type="text"
            placeholder="Enter custom time here"
            v-model="newWorkTime"
            @keyup.enter="setTime('work', newWorkTime)"
          />
          <input v-show="hidden" v-model="newWorkTime" />
        </div>
        <div>
          Break Timer:
          <input
            type="text"
            placeholder="Enter custom time here"
            v-model="newBreakTime"
            @keyup.enter="setTime('break', newBreakTime)"
          />
          <input v-show="hidden" v-model="newBreakTime" />
        </div>
      </form> -->
    </div>
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: "HomeView",
  data() {
    return {
      timerRunning: false,

      // For logic
      elapsedTime: 0,
      workTime: 50 * 60,
      breakTime: 10 * 60,

      // For display
      displayWorkTime: 50 * 60,
      displayBreakTime: 10 * 60,

      // For accepting user input
      newWorkTime: null,
      newBreakTime: null,

      // holds interval
      timer: 0,

      bellSound: new Audio(
        "https://cdn.freesound.org/previews/66/66952_634166-lq.mp3"
      ),
    };
  },

  methods: {
    startStop() {
      this.timerRunning ? this.stopTimer() : this.startTimer();
    },
    startTimer() {
      this.timer = setInterval(this.handleTimer, 1000);
      this.timerRunning = !this.timerRunning;
    },
    stopTimer() {
      clearInterval(this.timer);
      this.timerRunning = !this.timerRunning;
    },
    handleTimer() {
      this.elapsedTime = this.elapsedTime + 1;
      this.displayWorkTime = Math.max(0, this.workTime - this.elapsedTime);
      this.displayBreakTime = Math.min(
        this.breakTime,
        this.breakTime + this.workTime - this.elapsedTime
      );
      if (this.elapsedTime == this.workTime) {
        this.alertUser();
      }
      if (this.elapsedTime == this.workTime + this.breakTime) {
        this.alertUser();
        this.reset();
      }
    },
    setTime(timer, minutes) {
      if (timer == "work") {
        this.workTime = minutes * 60;
        this.displayWorkTime = this.workTime;
        this.newWorkTime = null;
      } else if (timer == "break") {
        this.breakTime = minutes * 60;
        this.displayBreakTime = this.breakTime;
        this.newBreakTime = null;
      }
    },
    reset() {
      console.log("Call to: reset");
      this.elapsedTime = 0;
      this.stopTimer();
      this.displayWorkTime = this.workTime;
      this.displayBreakTime = this.BreakTime;
    },
    alertUser() {
      this.bellSound.play();
    },
    formatTime(input_seconds) {
      var dateObj = new Date(Math.floor(input_seconds * 1000));
      var hours = dateObj.getUTCHours();
      var minutes = dateObj.getUTCMinutes();
      var seconds = dateObj.getSeconds();

      var o1 =
        input_seconds > 3600 ? hours.toString().padStart(2, "0") + ":" : "";
      var o2 =
        minutes.toString().padStart(2, "0") +
        ":" +
        seconds.toString().padStart(2, "0");
      return o1 + o2;
    },
  },
};
</script>

<style>
.container {
  display: flex;
  height: 90vh;
}

#timerContainer {
  text-align: center;
  margin: auto;
  font-family: "Courier New", Courier, monospace;
}

.timer {
  font-size: max(4rem, 12vh);
}

.row {
  display: flex;
  justify-content: center;
}

.button {
  background-color: var(--color2);
  border: solid;
  border-color: var(--color4);
  color: var(--color4);
  padding: 1rem;
  margin: 0.5rem;
  border-radius: 0.2rem;
  font-size: max(1rem, 3vh);
}

#start-stop {
  background-color: var(--color2);
  border-color: var(--color4);
}
</style>
