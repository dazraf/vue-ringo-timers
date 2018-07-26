<template>
  <span 
  v-bind:class="{ 'started': started, 'edit-mode': editMode }" 
  class='timer' v-on:click='enterEditMode'>{{hours | twoDigits}}h:{{minutes | twoDigits}}m:{{seconds | twoDigits }}s</span>
</template>
<script>
export default {
  name: "Timer",
  props: { 
    id: {
      type: Number,
      required: true
    }
  },
  data: () => {
    const now = Date.now();
    return {
      editMode: false,
      started: false,
      initial: "000005",
      previousInitial: "",
      startTime: now,
      currentTime: now,
      endTime: now,
      timerId: 0
    };
  },
  computed: {
    initialDuration() {
      const seconds = Number.parseInt(this.initial.slice(-2));
      const minutes = Number.parseInt(this.initial.slice(2, 4));
      const hours = Number.parseInt(this.initial.slice(0, 2));
      const ms = ((((hours * 60) + minutes) * 60) + seconds) * 1000;
      return ms;
    },
    diff() {
      if (this.started) {
        return (this.endTime - this.currentTime) / 1000;
      } else {
        return this.initialDuration / 1000;
      }
    },
    hours() {
      return Math.trunc((this.diff / 60 / 60) % 24);
    },
    minutes() {
      return Math.trunc((this.diff / 60) % 60);
    },
    seconds() {
      return Math.trunc(this.diff % 60);
    }
  },
  methods: {
    start() {
      this.editMode = false;
      this.started = true;
      const now = Date.now();
      this.currentTime = now;
      this.endTime = (now + this.initialDuration) + 999;
      this.timerId = setInterval(() => {
        const now = Date.now();
        if (this.endTime - now < 0) {
          this.stop();
        } else {
          this.currentTime = now;
        }
      }, 100);
    },
    stop() {
      this.started = false;
      clearInterval(this.timerId);
      this.$emit('finished', this);
    },
    enterEditMode() {
      if (this.started) return;
      this.$el.focus()
      this.editMode = true;
      this.previousInitial = this.initial;
      window.addEventListener('keypress', this.onKeyPress);
      this.initial = "000000";
    },
    leaveEditMode() {
      if (!this.editMode) return;
      this.editMode = false;
      window.removeEventListener('keypress', this.onKeyPress);
    },
    onKeyPress(e) {
      const key = e.key;
      if (key === 'Escape') {
        this.initial = this.previousInitial;
        this.leaveEditMode();
        return;
      } else if (key === 'Enter') {
        this.leaveEditMode();
      } else if (!isFinite(key)) {
        return;
      } else {
        this.initial = this.initial.substring(1) + key;
      }
    }
  },
  filters: {
    twoDigits(value) {
      if (value.toString().length <= 1) {
        return "0" + value.toString();
      } else {
        return value.toString();
      }
    }
  },
  mounted: function() {
  }
};
</script>

<style>
.edit-mode {
  color: gray
}
.timer {
  font-size: 40px;
}
.started {
  background-color: blueviolet;
  color: white;
}
</style>
