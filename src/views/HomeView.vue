<template>
  <div class="container">
    <div>Work timer:</div>
    <div>
      {{ formatTime(this.workTimerValue) }}
    </div>
    <div>Break timer:</div>
    <div>
      {{ formatTime(this.breakTimerValue) }}
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
          v-model="newWorkTimerValue"
          @keyup.enter="setTime('work', newWorkTimerValue)"
        />
        <input v-show="hidden" v-model="newWorkTimerValue" />
      </div>
      <div>
        Break Timer:
        <input
          type="text"
          placeholder="Enter custom time here"
          v-model="newBreakTimerValue"
          @keyup.enter="setTime('break', newBreakTimerValue)"
        />
        <input v-show="hidden" v-model="newBreakTimerValue" />
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
      workTimerValue: 50 * 60,
      newWorkTimerValue: null,
      breakTimerValue: 10 * 60,
      newBreakTimerValue: null,
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
      this.workTimerValue =
        this.workTimerValue > 0 ? this.workTimerValue - 1 : 0;
      if (this.workTimerValue == 0) {
        this.breakTimerValue =
          this.breakTimerValue > 0 ? this.breakTimerValue - 1 : 0;
      }
    },
    setTime(timer, minutes) {
      if (timer == "work") {
        this.workTimerValue = minutes * 60;
        this.newWorkTimerValue = null;
      } else if (timer == "break") {
        this.breakTimerValue = minutes * 60;
        this.newBreakTimerValue = null;
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
