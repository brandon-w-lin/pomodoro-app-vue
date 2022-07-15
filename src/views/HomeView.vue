<template>
  <div class="container">
    <div id="timer">
      <div class="row m-5">timer</div>
      <div class="row">start/stop button</div>
      <div class="row">
        <div class="col">
          <button
            @click="
              setTime('work', 50);
              setTime('break', 10);
            "
          >
            50 / 10
          </button>
        </div>
        <div class="col">
          <button
            @click="
              setTime('work', 45);
              setTime('break', 15);
            "
          >
            45 / 15
          </button>
        </div>
        <div class="col">
          <button
            @click="
              setTime('work', 25);
              setTime('break', 5);
            "
          >
            25 / 5
          </button>
        </div>
      </div>
    </div>
    <div>Work timer:</div>
    <div>
      {{ formatTime(this.displayWorkTime) }}
    </div>
    <div>Break timer:</div>
    <div>
      {{ formatTime(this.displayBreakTime) }}
    </div>
    <button @click="startStop()">Start / Stop</button>
    <!-- <button @click=""></button> -->
    Create custom timer:
    <form>
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
    </form>
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
    };
  },

  methods: {
    startStop() {
      this.timerRunning ? this.stopTimer() : this.startTimer();
      this.timerRunning = !this.timerRunning;
    },
    startTimer() {
      this.timer = setInterval(this.handleTimer, 1000);
    },
    stopTimer() {
      clearInterval(this.timer);
    },
    handleTimer() {
      this.elapsedTime = this.elapsedTime + 1;
      this.displayWorkTime = Math.max(0, this.workTime - this.elapsedTime);
      this.displayBreakTime = Math.min(
        this.breakTime,
        this.breakTime + this.workTime - this.elapsedTime
      );
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
    alertUser() {
      // make a noise here
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
#timer {
  border: solid;
  /* display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center; */
}
</style>
