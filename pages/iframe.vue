<template>
  <div style="background-color: #7F828B; min-height: 100vh">

    <h1>My iframe</h1>

    <IframeParent
      src="http://localhost:3000/"
      foo="bar"
      :i="['plop', 'test']"
      :modal="modal"
      @modal="modal = $event"
    />

    <div style="margin-top: 20px" v-if="modal">
      Je suis une modale avec pour info : {{modal}}
    </div>
  </div>
</template>

<script>
  import IframeParent from '../components/IframeParent'

  export default {
    layout: 'parent',
    data() {
        return {
            modal: null
        }
    },
    components: {
      IframeParent,
    },
  }
</script>
