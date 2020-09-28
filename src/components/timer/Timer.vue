<template>
  <div class="timer">
    <input
      class="timer__input"
      :class="{ active: isOpacity }"
      v-model="allTimes"
      disabled
    />
    <div class="timer__buttons">
      <div
        class="timer__button timer__button-start"
        v-if="!isActiveStopWatch"
        @click="start"
        :class="{ active: isOpacity }"
      ></div>
      <div
        class="timer__button timer__button-pause"
        v-if="isActiveStopWatch"
        @click="pause"
      ></div>
      <div
        class="timer__button timer__button-stop"
        @click="reset"
        :class="{ active: isOpacity }"
      ></div>
    </div>
  </div>
</template>

<script>
export default {
  props: ["time"],

  data() {
    return {
      allTimes: this.time,
      isActiveStopWatch: false,
      timer: null,
      seconds: null,
      minutes: null,
      hours: null,
      isOpacity: true
    };
  },

  methods: {
    start() {
      this.isOpacity = false;
      this.isActiveStopWatch = true;
      this.timer = setInterval(() => {
        const arrayFromStringAllTimes = this.allTimes.split(":").map(Number);

        this.stopWatch(arrayFromStringAllTimes);
        this.transformArrayToString([this.hours, this.minutes, this.seconds]);
      }, 1000);
    },

    stopWatch(array) {
      this.seconds = array[array.length - 1];
      this.minutes = array[array.length - 2];
      this.hours = array[array.length - 3];

      if (this.seconds < 59) {
        this.seconds++;
      } else {
        this.seconds = 0;
        if (this.minutes < 59 || !this.minutes) {
          this.minutes || this.minutes === 0
            ? this.minutes
            : (this.minutes = 0);
          this.minutes++;
        } else {
          this.minutes = 0;
          this.hours || this.hours === 0 ? this.hours : (this.hours = 0);
          this.hours++;
        }
      }
    },

    transformArrayToString(array) {
      const filterArray = array.reduce((acc, el) => {
        if (typeof el !== "undefined") {
          acc.push(el);
        }

        return acc;
      }, []);

      this.allTimes = filterArray.reduce((acc, el, idx, array) => {
        idx !== array.length - 1
          ? (el = `${array[idx]}:`)
          : (el = `${array[idx]}`);
        acc += el;

        return acc;
      }, "");
    },

    pause() {
      clearInterval(this.timer);
      this.isActiveStopWatch = false;
      this.isOpacity = true;
    },

    reset() {
      clearInterval(this.timer);
      this.allTimes = "0";
      this.isActiveStopWatch = false;
      this.isOpacity = true;
    }
  }
};
</script>

<style lang="scss" scoped>
.timer {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-bottom: 45px;
  width: 225px;
  background-color: #696969;

  &__input {
    display: flex;
    border: none;
    background-color: transparent;
    width: 100%;
    padding-top: 19px;
    padding-bottom: 16px;
    font-family: "GothamPro", "Arial", sans-serif;
    font-size: 22px;
    line-height: 21px;
    text-align: center;
    color: #ffffff;
    border-bottom: 1px solid #ffffff;
  }

  &__buttons {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-top: 20px;
    margin-bottom: 19px;
    width: 84px;
  }

  &__button {
    cursor: pointer;
  }

  &__button-start {
    border: 10px solid transparent;
    border-left: 15px solid #ffffff;
  }

  &__button-pause {
    width: 10px;
    height: 20px;
    border-left: 3px solid #ffffff;
    border-right: 3px solid #ffffff;
  }

  &__button-stop {
    width: 20px;
    height: 20px;
    background: #ffffff;
  }

  @media screen and (min-width: 1024px) {
    margin-right: 50px;
  }
}

.active {
  opacity: 0.3;
}
</style>