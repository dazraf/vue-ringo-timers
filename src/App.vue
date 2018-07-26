<template>
  <div id="app">
    <img src="https://images.eil.com/large_image/THE%5FBEATLES_YELLOW%2BSUBMARINE%2B%2D%2BRINGO%2BSTARR-324914.jpg" width="100">
    <h2>Ring 'o' Timers</h2>
    <h3>Instructions</h3>
    <div>Click <i>Add Timer</i> to add more timers. Click in the timers to set their value. Then press <i>Start</i> to set off The Ring 'o' Timers.</div>
    <hr>
    <button v-on:click="start()">Start</button><button v-on:click="timerCount++">Add Timer</button>
    <div v-for="id in this.timerCount"><Timer v-bind:ref="id" v-bind:id="id" v-on:finished="onFinished"/></div>
  </div>
</template>

<script>
import Timer from "./components/Timer.vue";

export default {
  name: "app",
  components: {
    Timer
  },
  data: function() {
    return {
      timerCount: 2
    };
  },
  methods: {
    start() {
      const timer = this.$refs[1][0];
      timer.start();
    },
    onFinished(timer) {
      const nextId = timer.id % this.timerCount + 1;
      this.$refs[nextId][0].start();
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
button {
  padding: 0.2em 1.45em;
  display: inline-block;
  margin: 0.1em;
  border: 0.15em solid #cccccc;
  box-sizing: border-box;
  text-decoration: none;
  font-family: "Segoe UI", "Roboto", sans-serif;
  font-weight: 400;
  font-size: 20px;
  color: #000000;
  background-color: #cccccc;
  text-align: center;
  position: relative;
}
button:hover {
  border-color: #7a7a7a;
}
button:active {
  background-color: #999999;
}
@media all and (max-width: 30em) {
   button {
    display: block;
    margin: 0.2em auto;
  }
}

hr {
  border: 0;
  height: 1px;
  background-image: linear-gradient(
    to right,
    rgba(0, 0, 0, 0),
    rgba(0, 0, 0, 0.75),
    rgba(0, 0, 0, 0)
  );
}
</style>
