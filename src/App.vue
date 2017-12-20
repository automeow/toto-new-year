<template>
  <div id="app" v-bind:class="{ 'weather rain': newYear }">
    <header>
      Africa New Year Count Down
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
      <h1 v-if="newYear">
        Have a blessed new year!
      </h1>
      <template v-else>
        <p>Blessing the rains will commence in...</p>
        <count-down
          :deadline="newYearDate"
          v-on:done="happyNewYear"></count-down>
        <count-down
          v-show="false"
          :deadline="totoDate"
          v-on:done="play"></count-down>
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
      totoDate: this.getTotoDate(),
      newYearDate: this.getNewYearDate(),
      newYear: false
    }
  },
  computed: {
    aplayer () {
      return this.$refs.player.control
    }
  },
  methods: {
    getCurrentYear () {
      return (new Date()).getFullYear()
    },
    getTotoDate () {
      const d = this.getNewYearDate()
      d.setTime(d.getTime() - 80000)
      return d
    },
    getNewYearDate () {
      if (window.location.hash === '#test') {
        const d = new Date()
        d.setTime(d.getTime() + 90000)
        return d
      }

      return new Date(this.getCurrentYear() + 1, 0, 1, 0, 0, 0)
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
    font-size: 2.4vw;

    header {
      padding: 3vw;
      margin-bottom: 6vw;
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
