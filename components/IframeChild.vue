<template>
  <div>
    <slot />
  </div>
</template>

<script>
  import postRobot from 'post-robot'

  export default {
    name: 'IframeChild',
    provide() {
      return {
        iframe: this.iframe,
      }
    },
    data() {
      return {
        iframe: {
          params: {},
          emit: this.postMessage
        },
      }
    },
    mounted() {
      postRobot.on('setData', (evt) => this.setDataFromParent(evt.data))

      const resizeObserver = new ResizeObserver(() => {
        // Can't use entry.contentRect because of polyfill misbehavior
        this.postMessage('iframeHeight', {offsetHeight: document.body.offsetHeight})
      })
      resizeObserver.observe(document.body)

      window.onpopstate = () => {
        this.postMessage('iframeUrl', { href: location.href })
      }

      this.postMessage('mounted')
    },
    methods: {
      postMessage(eventName, payload) {
        postRobot.send(parent.window, eventName, payload)
      },
      setDataFromParent(data) {
        this.iframe.params = data;
      },
    },
  }
</script>
