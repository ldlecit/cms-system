<template>
  <span class="comp-time-ticker">
    {{text}}<slot></slot>
  </span>
</template>

<script>

export default {
  name: 'TimeTicker',
  props: {
    duration: Number,
    interval: {
      type: Number,
      default: 1
    }
  },
  data () {
    return {
      $ticker: 0,
      durationMil: this.duration ? this.duration * 1000 : 0
    }
  },
  computed: {
    text () {
      const {durationMil} = this
      const duraSec = durationMil / 1000
      return `${durationMil % 1000 === 0 ? duraSec : duraSec.toFixed(1)}s`
    },
    tickerInterval () {
      return this.interval ? this.interval * 1000 : 1000
    }
  },
  mounted () {
    this.$ticker = this.createTicker()
  },
  beforeDestroy () {
    this.destroyTicker()
  },
  methods: {
    createTicker () {
      const {tickerInterval} = this
      return setInterval(() => {
        this.durationMil -= tickerInterval
        if (this.durationMil <= 0) {
          this.$emit('on-timeout')
          this.destroyTicker()
        }
      }, tickerInterval)
    },
    destroyTicker () {
      clearInterval(this.$ticker)
    }
  }
}
</script>
<style lang="scss" scoped>
.load-btn{

}
</style>
