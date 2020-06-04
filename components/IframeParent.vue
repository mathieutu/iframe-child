<template>
    <iframe
            :src="src"
            :style="{ height: `${iframeHeight}px` }"
            :name="name"
    />
</template>

<script>
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
      window.addEventListener('message', this.handleMessage)
    },
    destroyed() {
      window.removeEventListener('message', this.handleMessage)
    },
    watch: {
      $attrs() {
        this.updateParams()
      },
    },
    methods: {
      updateParams() {
        window.frames[this.name].postMessage({
          fromParent: true,
          type: 'setData',
          payload: this.$attrs,
        }, '*')
      },

      handleMessage({ data: { fromIframe, type, payload } }) {
        if (!fromIframe) {
          return
        }

        console.log({ fromIframe, type, payload });

        const actions = {
          mounted: this.updateParams,
          iframeHeight: height => this.iframeHeight = height,
          iframeUrl: url => this.iframeUrl = url,
        }

        actions[type]?.(payload)

        this.$emit(type, payload)
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
