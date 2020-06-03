<template>
  <div>
    <slot />
  </div>
</template>

<script>
  export default {
    name: 'IframeChild',
    provide() {
      return {
        iframe: {
          params: this.params,
          emit: (name, value) => this.postMessage({event: {name, value}}),
        },
      }
    },
    data() {
      return {
        params: this.$route.query,
      }
    },
    mounted() {
      const resizeObserver = new ResizeObserver(() => {
        // Can't use entry.contentRect because of polyfill misbehavior
        this.postMessage({ iframeHeight: document.body.offsetHeight })
      })
      resizeObserver.observe(document.body)
    },
    methods: {
      postMessage(message) {
        parent.window.postMessage(
          message,
          '*',
        )
      },
    },
    computed: {},
  }
</script>
