<template>
  <div id="app">
    <div class="stop-watch">
      <Timer v-for="count in timers" :key="count.id" :time="count.time"></Timer>
      <button
        class="stop-watch__append-timer"
        type="button"
        @click="appendTimer"
      >
        +
      </button>
    </div>
  </div>
</template>

<script>
import Timer from "./components/timer";

export default {
  name: "App",

  components: {
    Timer
  },

  data() {
    return {
      timers: [
        { id: 1, time: "2:40" },
        { id: 2, time: "25:32" },
        { id: 3, time: "1:30:15" },
        { id: 4, time: "1:20:33" }
      ]
    };
  },

  methods: {
    appendTimer() {
      this.timers.push({ id: this.timers.length + 1, time: "0" });
    }
  }
};
</script>

<style lang="scss">
@import "./base.scss";
#app {
  display: flex;
  justify-content: center;
  margin-top: 72px;
}

.stop-watch {
  display: flex;
  flex-direction: column;
  justify-content: center;

  &__append-timer {
    display: flex;
    justify-content: center;
    align-items: center;
    margin-bottom: 20px;
    width: 225px;
    height: 120px;
    font-family: "GothamPro", "Arial", sans-serif;
    font-size: 40px;
    color: #9e9e9e;
    background-color: #696969;
  }

  @media screen and (min-width: 768px) {
    flex-direction: unset;
    flex-wrap: wrap;
    justify-content: space-between;
    width: 500px;
  }

  @media screen and (min-width: 1024px) {
    width: 826px;
    justify-content: unset;
  }
}
</style>
