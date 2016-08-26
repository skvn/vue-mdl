<template lang="jade">
.mdl-dialog-container(v-show='show')
  .mdl-dialog(v-bind:class='{ "mdl-dialog__wide": wide, "mdl-dialog__50": half }')
    .mdl-dialog__title
      slot(name='title')
        {{title}}
    .mdl-dialog__content
      slot(name='content')
    .mdl-dialog__actions(v-bind:class='{ "mdl-dialog__actions--full-width": fullWidth }')
      slot(name='buttons')
        mdl-button.mdl-js-ripple-effect(v-on:click.stop='close') Close
</template>

<script>
import propFill from './mixins/prop-fill'
import mdlButton from './button.vue'

export default {
  components: {
    mdlButton
  },
  data () {
    return {
      show: false
    }
  },
  props: {
    title: {
      type: String
    },
    fullWidth: {
      fill: true,
      default: false
    },
    wide: {
      fill: true,
      default: false
    },
    half: {
      fill: true,
      default: false
    }
  },
  methods: {
    open () {
      this.show = true
      this.$emit('open')
    },
    close () {
      this.show = false
      this.$emit('close')
    }
  },
  mixins: [propFill]
}
</script>
<style>
.mdl-dialog-container {
  position: fixed;
  width: 100%;
  height: 100%;
  display: flex;
  flex-wrap: wrap;
  justify-content:center;
  align-items: center;
  top:0;
  left: 0;
  z-index: 10000;
  background: rgba(0,0,0,0.3);
}
.mdl-dialog-container .mdl-dialog {
  background-color:white;
  padding: 1em;
  color: black;
  width: initial;
  min-width: 500px;
}
.mdl-dialog__wide {
  width: 90% !important;
}
.mdl-dialog__50 {
  width: 50% !important;
}
</style>
