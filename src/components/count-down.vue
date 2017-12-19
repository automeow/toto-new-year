<template>
  <dl class="count-down">
    <dt>Days</dt>
    <dd>{{ days }}</dd>
    <dt>Hours</dt>
    <dd>{{ hours }}</dd>
    <dt>Minutes</dt>
    <dd>{{ minutes }}</dd>
    <dt>Seconds</dt>
    <dd>{{ seconds }}</dd>
  </dl>
</template>

<script>

export default {
  name: 'count-down',
  props: {
    deadline: {
      type: Date,
      required: true
    }
  },
  data () {
    return {
      remaining: this.getTimeRemaining()
    }
  },
  computed: {
    seconds () {
      return this.remaining % 60
    },
    minutes () {
      return Math.floor(this.remaining / 60) % 60
    },
    hours () {
      return Math.floor(this.remaining / (60 * 60)) % 24
    },
    days () {
      return Math.floor(this.remaining / (60 * 60 * 24))
    }
  },
  watch: {
    deadline () {
      this.setInterval()
    }
  },
  mounted () {
    this.setInterval()
  },
  destroyed () {
    clearTimeout(this.timer)
  },
  methods: {
    getTimeRemaining () {
      return Math.max(0, Math.floor((this.deadline - new Date()) / 1000))
    },
    setInterval () {
      if (this.timer) clearTimeout(this.timer)

      this.remaining = this.getTimeRemaining()

      this.timer = setInterval(() => {
        this.remaining = this.getTimeRemaining()

        if (this.remaining === 0) {
          clearTimeout(this.timer)
          this.$emit('done')
        }
      }, 1000)
    }
  }
}
</script>

<style lang="less">
.count-down {
  display: inline-grid;
  grid-template-columns: repeat(4, 1fr);

  dt, dd {
    margin: 0;
    padding: 0.1em 0.5em;
    font-size: 1.8em;
  }

  dt {
    grid-row: 1;
    font-weight: bold;
  }

  dd {
    grid-row: 2;
  }
}
</style>
