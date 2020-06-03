<template>
    <iframe
      :src="url"
      :style="{ height: `${iframeHeight}px` }"
       />
</template>

<script>
  import * as qs from 'qs'

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
        iframeHeight: null
      };
    },
    mounted() {
      window.addEventListener("message", this.handleMessage);
    },
    destroyed() {
      window.removeEventListener("message", this.handleMessage);
    },
    computed: {
      url() {
        const url = new URL(this.src)

        const params = qs.stringify({
          ...qs.parse(url.search),
          ...this.$attrs,
        })

        url.search = params

        return url.toString()
      },
    },
    methods: {
      handleMessage({ data }) {
        if (data.iframeHeight) {
          this.iframeHeight = data.iframeHeight;
        }

        if (data.event) {
          this.$emit(data.event.name, data.event.value)
        }
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
