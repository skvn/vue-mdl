<template>
  <input class='vue-scrubber' v-model='constrainedValue' @mousedown='handleMouseDown' @keypress='handleInput'
         @keydown.up='handleKeyCodeUp' @keydown.down='handleKeyCodeDown' @change='handleChange'/>
</template>

<script>
  export default {
    data: function () {
      return {
        isMouseDown: false,
        initialMouse: null,
        min: 0,
        max: 100,
        step: 1,
        value: 0
      }
    },
    computed: {
// returns the number of decimals based on the step value
// e.g. "0.25" returns "2"
      decimals: function () {
        return this.step.toString().substr((this.step).toString().indexOf(".")).length - 1;
      },

// every time the value changes, we need to make sure it stays inside the min/max
      constrainedValue: function () {
        return this.constrain(this.value, this.min, this.max, this.decimals);
      }
    },

    // props that the scrubber can receive
    // value: initial value
    // min: minimum value
    // max: maximum value
    // step: increments for each pixel the mouse is moved
    props: ["value", "min", "max", "step"],
    methods: {

// constrains a number to not exceed the min/max
// decimals: rounding precision
      constrain: function (value, min, max, decimals) {
        decimals = typeof decimals !== 'undefined' ? decimals : 0;

        if (min != undefined && max != undefined) {
          return this.round(Math.min(Math.max(parseFloat(value), min), max), decimals);
        } else {
          return value;
        }
      },

// method to round a number to given decimals
      round: function (value, decimals) {
        return Number(Math.round(value + 'e' + decimals) + 'e-' + decimals);
      },
      handleInput: function (event) {
// only allow numeric keys
        if (event.keyCode < 48 || event.keyCode > 57) {
          event.preventDefault();
        }
      },

      handleChange: function (event) {

        this.value = isNaN(parseFloat(event.target.value)) ? 0 : parseFloat(event.target.value);
      },

      handleKeyCodeUp: function (event) {
        event.preventDefault();
        this.value += this.step;
      },

      handleKeyCodeDown: function (event) {
        event.preventDefault();
        this.value -= this.step;
      },

// mouse handler
      handleMouseDown: function (event) {

// enable scrubbing
        this.mouseDown = true;

// remember the initial mouse position when the scubbing started
        this.initialMouse = {
          x: event.clientX,
          y: event.clientY
        }

// remember the initial value
        this.initialValue = this.value;

        var that = this;

// register global event handlers because now we are not bound to the component anymore
        document.onmousemove = function (event) {
          that.handleMouseMove(event)
        }

// global mouse up listener
        document.onmouseup = function (event) {
          that.handleMouseUp(event)
        }
      },
      handleMouseUp: function ($event) {

// disable scrubbing
        this.mouseDown = false;

      },

// the actual translation of mouse movement to value change…
      handleMouseMove: function (event) {

// scrub if the mouse is being pressed
        if (this.mouseDown) {
          var newValue = this.initialValue + ((event.clientX - this.initialMouse.x) * this.step)

// constrain the value to the min/max
          this.value = this.constrain(newValue, this.min, this.max, this.decimals);

        }
      }
    }
  }
</script>