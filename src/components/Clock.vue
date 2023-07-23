<template>
  <div class="main" :style="styleData">
    <div class="point hour" />
    <div class="point minute" />
  </div>
</template>

<script>
export default {
  data() {
    return {
      hour: 0,
      hourRate: 0,
      minute: 0,
      minuteRate: 0,
      hourTransitionTime: 0,
      minuteTransitionTime: 0,
      speed: {
        hourBaseSpeed: 215,
        minuteBaseSpeed: 315,
        hourSpeedRate: 1,
        minuteSpeedRate: 1,
      }
    }
  },
  props: {
    ClockSize: {
      type: Number,
      default: 200
    },
  },
  computed: {
    styleData() {
      return {
        '--hourRate': Math.round(this.hourRate) + 'deg',
        '--hourTransitionTime': this.hourTransitionTime + 's',
        '--minuteRate': Math.round(this.minuteRate) + 'deg',
        '--minuteTransitionTime': this.minuteTransitionTime + 's',
      }
    },
    hourSpeed() {
      return this.speed.hourBaseSpeed * this.speed.hourSpeedRate
    },
    minuteSpeed() {
      return this.speed.minuteBaseSpeed * this.speed.minuteSpeedRate
    }
  },
  methods: {
    setHourRate(value) {
      if (value >= 0 & value <= 360) {
        var valueError = Math.abs(this.minuteRate - value)
        this.hourTransitionTime = valueError / this.hourSpeed
        this.hour = value / 360 * 24
        this.hourRate = value
      }
    },
    setMinuteRate(value) {
      if (value >= 0 & value <= 360) {
        var valueError = Math.abs(this.minuteRate - value)
        if (valueError > 180) {
          if (this.minuteRate > value) {
            this.setMinuteRate(value + 360)
            return
          }
        }
        this.minuteTransitionTime = valueError / this.minuteSpeed
        this.minute = value / 360 * 60
        this.minuteRate = value
      }
    },
    setHour(value) {
      if (value >= 0 & value <= 24) {
        var hourRate = Math.round(value / 24 * 360)
        this.setHourRate(hourRate)
      } else {
        alert(`ValueError：This value(${value}) does not match hour type.`)
      }
    },
    setMinute(value) {
      if (value >= 0 & value <= 60) {
        var minuteRate = Math.round(value / 60 * 360)
        this.setMinuteRate(minuteRate)
      } else {
        alert(`ValueError：This value(${value}) does not match minute type.`)
      }
    },
    setHourSpeedRate(rate = 1) {
      this.speed.hourSpeedRate = rate
    },
    setMinuteSpeedRate(rate = 1) {
      this.speed.minuteSpeedRate = rate
    },
    setSpeedRate(rate = 1) {
      this.setHourSpeedRate(rate)
      this.setMinuteSpeedRate(rate)
    }
  }
}
</script>



<style lang="scss" scoped>
$hourLength: 36;
$minuteLength: 48;
$pointBackground: black;
$size: 9vh;
.main {
  border-radius: 50%;
  background: linear-gradient(145deg, #d8d8d8, #ffffff);
  box-shadow: 20px 20px 60px #cccccc, -20px -20px 60px #ffffff;
  height: $size;
  width: $size;
  position: absolute;
  .point {
    position: absolute;
    background-image: linear-gradient($pointBackground, $pointBackground);
    background-repeat: no-repeat;
    &.hour {
      background-position: center (60-$hourLength) * 1%;
      background-size: 100% $hourLength * 1%;
      height: 100%;
      width: 6%;
      margin: 0 47%;
      transform: rotateZ(var(--hourRate));
      transition: transform var(--hourTransitionTime);
    }
    &.minute {
      background-position: center 14%;
      background-size: 100% 45%;
      height: 100%;
      width: 6%;
      margin: 0 47%;
      transform: rotateZ(var(--minuteRate));
      transition: transform var(--minuteTransitionTime);
    }
  }
}
</style>