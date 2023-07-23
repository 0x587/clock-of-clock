<template>
  <div id="app">
    <div class="grid">
      <div class="border border-top border-horizontal" v-if="false">
        <div class="clock-box" v-for="(item, index) in 14" :key="index">{{item}}</div>
      </div>

      <div class="border border-down border-horizontal" v-if="false">
        <div class="clock-box" v-for="(item, index) in 14" :key="index">{{item}}</div>
      </div>
      <div class="border border-right border-vertical" v-if="false">
        <div class="clock-box" v-for="(item, index) in 8" :key="index">{{item}}</div>
      </div>
      <div class="border border-left border-vertical" v-if="false">
        <div class="clock-box" v-for="(item, index) in 8" :key="index">{{item}}</div>
      </div>

      <div :class="borderClass(side)" v-for="(side, index) in borderSide" :key="index">
        <div
          class="clock-box"
          v-for="(item, index) in ['left', 'right'].indexOf(side) != -1 ? 8 : 14"
          :key="index"
        >
          <clock :ref="`border-${side}-${item}`"></clock>
        </div>
      </div>

      <div class="char-box">
        <div v-for="(charIndex, index) in 4" :key="index" :class="['char','char-'+charIndex]">
          <div
            class="clock-box"
            v-for="(item, index) in 3*6"
            :key="index"
            :id="`char-${charIndex}-${item}`"
          >
            <clock :ref="`char-${charIndex}-${item}`"></clock>
          </div>
        </div>

        <div class="char dot">
          <div class="clock-box" v-for="(item, index) in 2*6" :key="index">
            <clock :ref="`char-dot-${item}`"></clock>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Clock from '@/components/Clock'

export default {
  name: 'App',
  components: {
    Clock
  },
  data() {
    return {
      flag: false
    }
  },
  mounted() {
    // set clock speed rate
    Object.keys(this.$refs).forEach((ele) => {
      if (ele.indexOf('border') != -1) {
        this.$refs[ele][0].setSpeedRate(0.6)
      } else if (ele.indexOf('char-3') != -1) {
        this.$refs[ele][0].setSpeedRate(0.7)
      } else {
        this.$refs[ele][0].setSpeedRate(0.4)
      }
    })
    // borer clock set
    setTimeout(() => {
      Object.keys(this.$refs).forEach((ele) => {
        if (ele.indexOf('border') != -1) {
          this.$refs[ele][0].setHourRate(45 + 90)
          this.$refs[ele][0].setMinuteRate(45 + 90)
        }
      })
    }, 100)
    setInterval(
      () => {
        var now = new Date()
        var hourBits = now.getHours().toString().split('')
        var MinuteBits = now.getMinutes().toString().split('')
        if (hourBits.length < 2) {
          hourBits.unshift('0')
        }
        if (MinuteBits.length < 2) {
          MinuteBits.unshift('0')
        }
        for (let i = 1; i <= 3 * 6; i++) {
          this.$refs['char-1-' + i][0].setHourRate(this.renderNumber(hourBits[0])[i - 1][0])
          this.$refs['char-1-' + i][0].setMinuteRate(this.renderNumber(hourBits[0])[i - 1][1])
          this.$refs['char-2-' + i][0].setHourRate(this.renderNumber(hourBits[1])[i - 1][0])
          this.$refs['char-2-' + i][0].setMinuteRate(this.renderNumber(hourBits[1])[i - 1][1])
          this.$refs['char-3-' + i][0].setHourRate(this.renderNumber(MinuteBits[0])[i - 1][0])
          this.$refs['char-3-' + i][0].setMinuteRate(this.renderNumber(MinuteBits[0])[i - 1][1])
          this.$refs['char-4-' + i][0].setHourRate(this.renderNumber(MinuteBits[1])[i - 1][0])
          this.$refs['char-4-' + i][0].setMinuteRate(this.renderNumber(MinuteBits[1])[i - 1][1])
        }
      }, 1000
    )
    setInterval(
      () => {
        if (this.flag) {
          for (let i = 1; i <= 2 * 6; i++) {
            this.$refs['char-dot-' + i][0].setHourRate(this.renderNumber('dot-tik')[i - 1][0])
            this.$refs['char-dot-' + i][0].setMinuteRate(this.renderNumber('dot-tik')[i - 1][1])
          }
          this.flag = !this.flag
        }
        else {
          for (let i = 1; i <= 2 * 6; i++) {
            this.$refs['char-dot-' + i][0].setHourRate(this.renderNumber('dot-tok')[i - 1][0])
            this.$refs['char-dot-' + i][0].setMinuteRate(this.renderNumber('dot-tok')[i - 1][1])
          }
          this.flag = !this.flag
        }
      }, 1000
    )
  },
  computed: {
    borderSide() {
      return ['left', 'right', 'top', 'down']
    },

  },
  methods: {
    renderNumber(value) {
      switch (value) {
        case '0':
          return [
            [180, 90], [270, 90], [270, 180],
            [180, 0], [180, 180], [180, 0],
            [180, 0], [180, 0], [180, 0],
            [180, 0], [180, 0], [180, 0],
            [180, 0], [0, 0], [180, 0],
            [0, 90], [270, 90], [270, 0],
          ]
        case '1':
          return [
            [225, 225], [180, 90], [270, 180],
            [225, 225], [180, 0], [0, 180],
            [225, 225], [180, 0], [0, 180],
            [225, 225], [180, 0], [0, 180],
            [225, 225], [180, 0], [0, 180],
            [225, 225], [90, 0], [0, 270],
          ]
        case '2':
          return [
            [90, 180], [90, 270], [180, 270],
            [0, 90], [270, 180], [180, 0],
            [90, 180], [0, 270], [180, 0],
            [0, 180], [180, 90], [270, 0],
            [0, 180], [90, 0], [180, 270],
            [0, 90], [270, 90], [270, 0],
          ]
        case '3':
          return [
            [180, 90], [270, 90], [270, 180],
            [90, 0], [180, 270], [0, 180],
            [180, 90], [270, 0], [0, 180],
            [90, 0], [180, 270], [0, 180],
            [180, 90], [270, 0], [0, 180],
            [90, 0], [90, 270], [0, 270],
          ]
        case '4':
          return [
            [180, 90], [270, 180], [270, 180],
            [180, 0], [0, 180], [0, 180],
            [180, 0], [0, 90], [0, 180],
            [90, 0], [180, 270], [0, 180],
            [225, 225], [180, 0], [0, 180],
            [225, 225], [90, 0], [0, 270],
          ]
        case '5':
          return [
            [180, 90], [270, 90], [270, 180],
            [180, 0], [90, 180], [0, 270],
            [180, 0], [0, 90], [270, 180],
            [90, 0], [180, 270], [0, 180],
            [180, 90], [270, 0], [0, 180],
            [90, 0], [90, 270], [0, 270],
          ]
        case '6':
          return [
            [180, 90], [270, 90], [270, 180],
            [180, 0], [90, 180], [0, 270],
            [180, 0], [0, 90], [270, 180],
            [180, 0], [180, 180], [0, 180],
            [180, 0], [0, 0], [0, 180],
            [90, 0], [90, 270], [0, 270],
          ]
        case '7':
          return [
            [180, 90], [270, 90], [270, 180],
            [90, 0], [180, 270], [0, 180],
            [225, 225], [225, 0], [0, 225],
            [180, 45], [45, 180], [225, 225],
            [180, 0], [0, 180], [225, 225],
            [90, 0], [0, 270], [225, 225],
          ]
        case '8':
          return [
            [180, 90], [270, 90], [270, 180],
            [180, 0], [180, 180], [180, 0],
            [135, 0], [0, 0], [225, 0],
            [180, 45], [180, 180], [180, 315],
            [180, 0], [0, 0], [180, 0],
            [0, 90], [270, 90], [270, 0],
          ]
        case '9':
          return [
            [180, 90], [270, 90], [270, 180],
            [180, 0], [180, 180], [0, 180],
            [0, 180], [0, 0], [0, 180],
            [0, 90], [180, 270], [0, 180],
            [180, 90], [0, 270], [0, 180],
            [0, 90], [90, 270], [0, 270],
          ]
        case 'dot-tik':
          return [
            [225, 225], [225, 225],
            [180, 90], [270, 180],
            [0, 90], [270, 0],
            [180, 90], [270, 180],
            [0, 90], [270, 0],
            [225, 225], [225, 225],
          ]
        case 'dot-tok':
          return [
            [225, 225], [225, 225],
            [135, 135], [225, 225],
            [45, 45], [315, 315],
            [135, 135], [225, 225],
            [45, 45], [315, 315],
            [225, 225], [225, 225],
          ]
        default:
          return this.renderNumber('0')
      }
    },
    borderClass(side) {
      return [
        'border',
        'border-' + side,
        'border-' + (['left', 'right'].indexOf(side) != -1 ? 'vertical' : 'horizontal')
      ]
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: Helvetica;
  .grid {
    display: grid;
    grid-template-columns: repeat(16, 1fr);
    grid-template-rows: repeat(8, 1fr);
    height: 96vh;
    .border {
      display: grid;
      .clock-box {
        align-items: center;
        justify-content: center;
        display: flex;
      }
    }
    .border-horizontal {
      grid-template-columns: repeat(14, 1fr);
      grid-template-rows: 1fr;
    }
    .border-vertical {
      grid-template-columns: 1fr;
      grid-template-rows: repeat(8, 1fr);
    }
    .border-top {
      grid-area: 1 / 2 / 2 / 16;
    }
    .border-down {
      grid-area: 8 / 2 / 9 / 16;
    }
    .border-left {
      grid-area: 1 / 1 / 9 / 2;
    }
    .border-right {
      grid-area: 1 / 16 / 9 / 17;
    }
    .char-box {
      display: grid;
      grid-template-columns: repeat(2, 3fr) 2fr repeat(2, 3fr);
      grid-template-rows: 1fr;
      grid-area: 2 / 2 / 8 / 16;
      .char {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        grid-template-rows: repeat(6, 1fr);
        .clock-box {
          align-items: center;
          justify-content: center;
          display: flex;
        }
      }
      .char-1 {
        grid-area: 1 / 1 / 2 / 2;
      }
      .char-2 {
        grid-area: 1 / 2 / 2 / 3;
      }
      .char-3 {
        grid-area: 1 / 4 / 2 / 5;
      }
      .char-4 {
        grid-area: 1 / 5 / 2 / 6;
      }
      .dot {
        display: grid;
        grid-template-columns: repeat(2, 1fr);
        grid-template-rows: repeat(6, 1fr);
        grid-area: 1 / 3 / 2 / 4;
      }
    }
  }
}
</style>
