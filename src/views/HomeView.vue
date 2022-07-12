<template>
  <div class="container">
    {{ formatTime(this.timerValue) }}
    <button @click="startStop()">Start / Stop</button>

    <!-- <button @click=""></button> -->
    Create custom timer:
    <form>
      <div>
        Work Timer:
        <input
          type="text"
          placeholder="Enter custom time here"
          v-model="newTimerValue"
          @keyup.enter="setTime(newTimerValue)"
        />
        <input v-show="hidden" v-model="newTimerValue" />
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
      timerValue: 50 * 60,
      newTimerValue: null,
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
      this.timerValue = this.timerValue > 0 ? this.timerValue - 1 : 0;
      if (this.timerValue == 0) {
        this.alertUser();
      }
    },
    setTime(minutes) {
      this.timerValue = minutes * 60;
      this.newTimerValue = null;
    },
    alertUser() {
      // make a noise here
    },
    formatTime(input_seconds) {
      var dateObj = new Date(Math.floor(input_seconds * 1000));
      var hours = dateObj.getUTCHours();
      var minutes = dateObj.getUTCMinutes();
      var seconds = dateObj.getSeconds();

      var o1 = input_seconds > 3600 ? hours.toString().padStart(2, "0") + ":" : "";
      var o2 = minutes.toString().padStart(2, "0") + ":" + seconds.toString().padStart(2, "0");
      return o1 + o2;
    },
  },
};

// Documentation:
// Keep track of timer state in variable timerRunning
// toggle timer with toggleTimer
// while toggleTimer, run interval to decrease time
// watcher to make noise when timer hits zero
</script>
