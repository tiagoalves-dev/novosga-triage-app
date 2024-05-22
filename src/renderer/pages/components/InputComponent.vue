<template>
    <div class="field">
      <label for="" class="label">
        {{ inputName }}
      </label>
      <div class="control">
        <input
          class="input is-medium"
          :id="inputName"
          :value="inputValue"
          :placeholder="'Input: '+ inputName + inputValue"
          @focus="onInputFocus"
          @input="onInputChange"
        >
      </div>
    </div>
  </template>
  
  <script>
  export default {
    name: 'Input',
    props: {
      inputs: {
        type: Object
      },
      inputName: {
        type: String
      }
    },
    methods: {
      onInputChange (input) {
        this.$emit('onInputChange', input)
      },
      onInputFocus (input) {
        this.$emit('onInputFocus', input)
      }
    },
    computed: {
      inputValue: function () {
        return this.inputs[this.inputName]
        // É o valor digitado no teclado virtual
      }
    }
  }
  </script>
  
  <!-- Add "scoped" attribute to limit CSS to this component only -->
  <style scoped>
  </style>