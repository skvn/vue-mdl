<template>
<span class="mdl-chip mdl-chip--deletable">
    <span class="mdl-chip__text"><slot></slot></span>
    <button v-if="icon" type="button" class="mdl-chip__action"><i class="material-icons">{{icon}}</i></button>
</span>
</template>

<script>
export default {

  props: {
    icon: {
      type: [String, Boolean],
      default: false
    },
    click: {}
  }
}
</script>
