<template>
    <iframe
            :src="src"
            :style="{ height: `${iframeHeight}px` }"
            :name="name"
    />
</template>

<script>
  import postRobot from 'post-robot'

  export default {
    name: 'IframeParent',
    inheritAttrs: false,
    props: {
      src: {
        type: String,
        required: true,
      },
    },
    data() {
      return {
        name: Math.random().toString(36).substring(7),
        iframeHeight: null,
        iframeUrl: this.src,

      }
    },
    mounted() {
      postRobot.on('mounted', () => this.handleIFrameMounted())
      postRobot.on('modal', (evt) => this.handleModal(evt.data))
      postRobot.on('iframeHeight', (evt) => this.handleIframeHeight(evt.data.height))
    },
    watch: {
      $attrs() {
        this.updateParams()
      },
    },
    methods: {
      updateParams() {
        postRobot.send(window.frames[this.name], 'setData', { ...this.$attrs })
      },
      handleIFrameMounted() {
        this.updateParams()
        this.$emit('iframeHeight')
      },
      handleIframeHeight(height) {
        this.iframeHeight = height
        this.$emit('iframeHeight', height)
      },

      handleModal(type) {
        this.$emit('modal', type)
      },
    },

  }
</script>

<style scoped>
    iframe {
        width: 100%;
        border: none;
    }
</style>
