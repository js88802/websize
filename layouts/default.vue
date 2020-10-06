<template>
  <div id="app">
    <GlobalHeader />
    <Nuxt />
  </div>
</template>

<script>
import GlobalHeader from '@/components/GlobalHeader'
export default {
  components: {
    GlobalHeader
  },
  data () {
    return {
      isPC: {
        state: true
      }
    }
  },
  provide () {
    return {
      isPC: this.isPC
    }
  },
  mounted () {
    window.addEventListener('resize', this.windowResizeEvent)
  },
  beforeDestroy () {
    window.removeEventListener('resize', this.windowResizeEvent)
  },
  methods: {
    getWindowWidth () {
      const width = document.documentElement.offsetWidth
      return width
    },
    windowResizeEvent () {
      const width = this.getWindowWidth()
      if (width < 744) {
        this.isPC.state = false
      } else {
        this.isPC.state = true
      }
    }
  }
}
</script>

<style lang="less">
#app {
  padding-top: 80px;
}

.ant-modal-body {
  padding: 0;
  background-color: transparent;
}
</style>
