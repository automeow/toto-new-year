<template>
  <div id="app" v-bind:class="{ 'weather rain': newYear }">
    <header>
      Africa - Toto New Year Count Down
    </header>
    <section>
      <a-player
        v-show="false"
        ref="player"
        mode="single"
        :music="{
          title: 'Africa',
          author: 'Toto',
          url: 'static/africa.mp3',
          pic: 'static/africa.jpg'
        }"></a-player>
      <template v-if="totoDone">
        <h1 v-if="newYear">
          Have a blessed new year!
        </h1>
        <template v-else>
          <p>Blessing the rains in...</p>
          <count-down
            :deadline="newYearDate"
            v-on:done="happyNewYear"></count-down>
        </template>
      </template>
      <template v-else>
        <p>Africa - Toto will begin playing in...</p>
        <count-down
          :deadline="totoDate"
          v-on:done="onTotoCountDown"></count-down>
      </template>
    </section>
  </div>
</template>

<script>
import CountDown from './components/count-down'
import APlayer from 'vue-aplayer'

export default {
  name: 'app',
  components: {
    CountDown,
    APlayer
  },
  data () {
    return {
      totoDone: false,
      totoDate: this.getTotoDate(),
      newYear: false
    }
  },
  computed: {
    aplayer () {
      return this.$refs.player.control
    }
  },
  created () {
    window.testRun = () => {
      this.totoDate = new Date()
    }
  },
  methods: {
    getCurrentYear () {
      return (new Date()).getFullYear()
    },
    getTotoDate () {
      return new Date(this.getCurrentYear(), 11, 31, 23, 58, 40)
    },
    getNewYearDate () {
      const d = new Date()
      d.setTime(d.getTime() + 81000)
      return d
    },
    onTotoCountDown () {
      this.totoDone = true
      this.newYearDate = this.getNewYearDate()
      this.play()
    },
    play () {
      this.aplayer.play()
    },
    happyNewYear () {
      this.newYear = true
    }
  }
}
</script>

<style lang="less">
@import './weather.css';

body {
  margin: 0;
  padding: 0;

  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    font-smoothing: antialiased;
    text-align: center;
    color: #2c3e50;
    min-height: 100vh;

    header {
      padding: 30px;
      margin-bottom: 60px;
      background: #333;
      color: #ddd;
      font-size: 5vw;
    }

    section {
      margin: 0 5vw;
    }
  }
}
</style>
