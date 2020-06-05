<template>
  <div style="background-color: #7F828B; min-height: 100vh">

    <h1>My iframe</h1>
    <label for="foo">Foo:</label>
    <input v-model="foo" id="foo"/>
    <IframeParent
      src="/iframe"
      :foo="foo"
      :i="['plop', 'test']"
      :modal="modal"
      @modal="modal = $event"
    />

<!--    <IframeParent-->

<!--      src="https://vibrant-blackwell-b21045.netlify.app/"-->
<!--      :foo="foo"-->
<!--      :i="['plop', 'test']"-->
<!--      :modal="modal"-->
<!--      @modal="modal = $event"-->
<!--    />-->

    <div style="margin-top: 20px" v-if="modal">
      Je suis une modale avec pour info : {{ modal }}
    </div>
    <!--      src="https://vibrant-blackwell-b21045.netlify.app/"-->
  </div>
</template>

<script>
  import IframeParent from '../components/IframeParent'

  export default {
    layout: 'parent',
    data() {
        return {
            foo: 'bar',
            modal: null
        }
    },
    components: {
      IframeParent,
    },
  }
</script>
