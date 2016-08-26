<style>
.getmdl-select .mdl-icon-toggle__label {
  float:right;
  margin-top:-30px;
  color: rgba(0, 0, 0, 0.4);
}

.getmdl-select.is-focused .mdl-icon-toggle__label {
  color: #3f51b5;
}

.getmdl-select .mdl-menu__container {
  width: 100% !important;
}
.getmdl-select .mdl-menu__container .mdl-menu {
  width: 100%;
}
</style>

<template lang="jade">
.mdl-textfield.mdl-js-textfield.mdl-textfield--floating-label.getmdl-select(v-el:textfield)
  input.mdl-textfield__input(v-bind:id.once='id', v-el:input, v-model='name', type='text', readonly='', tabindex='-1')
  label(v-bind:for.once='id')
    i.mdl-icon-toggle__label.material-icons keyboard_arrow_down
  label.mdl-textfield__label(v-bind:for.once='id') {{label}}
  ul.mdl-menu.mdl-menu--bottom-left.mdl-js-menu(v-bind:for.once='id')
    li.mdl-menu__item(v-for='(k,val) in options', v-on:click='selectValue(k)') {{val}}
</template>

<script>
export default {
  data () {
    return {
      name: ''
    }
  },
  methods: {
    selectValue (key) {
      this.value = key
      this.name = this.options[key]
      let event = new Event('change')
      this.$el.dispatchEvent(event)
    },
    setName () {
      this.name = null
      if (this.options && this.value in this.options) {
        this.name = this.options[this.value]
      }
      this.$els.textfield.MaterialTextfield.change(this.name)
      this.$els.textfield.MaterialTextfield.boundBlurHandler()
    },
    ensureId () {
      if (!this.id) {
        this.id = makeid()
      }
    }
  },

  props: {
    label: String,
    id: {
      required: false
    },
    value: {
      required: false
    },
    options: {
      required: true
    }
  },
  created () {
    this.ensureId()
  },
  ready () {
    componentHandler.upgradeElements(this.$el)
    this.setName()
  },
  watch: {
    value () {
      this.setName()
    }
  }
}

function makeid () {
  let text = ''
  const possible = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789'

  for (let i = 0; i < 5; i++) {
    text += possible.charAt(Math.floor(Math.random() * possible.length))
  }
  return text
}

</script>
