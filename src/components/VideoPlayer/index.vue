<template>
  <div class="comp-video-palyer">
    <video ref="video" class="video-js vjs-customized"></video>
    
  </div>
</template>

<script>
import videojs from 'video.js'
import 'video.js/dist/video-js.min.css'
const defOpts = {
  controls: true,
  preload: 'auto',
  techCanOverridePoster: true
}

export default {
  name: 'VideoPlayer',
  components: {
  },
  props: {
    options: {
      type: Object,
      default: () => ({})
    },
    src: String,
    poster: String
  },
  data () {
    return {
      $player: {}
    }
  },
  created () {
  },
  mounted () {
    this.$player = videojs(this.$refs.video, this.initOpts, function () {

    })
  },
  computed: {
    size () {
      const {offsetWidth: width, offsetHeight: height} = this.$el || {}
      return {width, height}
    },
    initAttrs () {
      return Object.assign({}, defAttrs, this.$attrs)
    },
    initOpts () {
      const {src} = this
      const sources = [{
        src,
        type: 'video/mp4'
      }]
      const poster = this.poster
      return Object.assign(defOpts, this.options, this.size, {sources, poster})
    }
  },
  watch: {
    src () {
      this.$player.src({src: this.src})
    }
  },
  methods: {
    // utils
    getVideoFirstFrame () {
      const video = this.$refs.video
      let canvas = document.createElement('canvas')
      Object.assign(canvas, this.size)
      if (canvas.getContext) {
        const ctx = canvas.getContext('2d')
        ctx.drawImage(video, 0, 0, this.size.width, this.size.height)
        return canvas.toDataURL('image/png')
      }
    }
    // bussiness
    // events
  },
  beforeDestroy () {
    this.$player.dispose && this.$player.dispose()
  }
}
</script>
<style lang="scss" scoped>
.comp-video-palyer {
  width: 100%;
  height: 100%;
  overflow: hidden;
  video {
    width: 100%;
    height: 100%;
  }
  /deep/ .vjs-customized{
    &:hover{
      .vjs-big-play-button{
        background-color: rgba(43,51,63,.7);
      }
      .vjs-icon-placeholder{
        color: #FE5435;
      }
    }
    .vjs-big-play-button{
      width: 60px;
      height: 60px;
      border: none;
      border-radius: 50%;
      font-size: 34px;
      top: 50%;
      left: 50%;
      margin-left: -30px;
      margin-right: -30px;
    }
   .vjs-icon-placeholder{
      line-height: 60px;
    }
  }
}
</style>