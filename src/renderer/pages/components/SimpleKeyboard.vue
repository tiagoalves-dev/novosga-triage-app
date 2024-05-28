<template>
  <!-- <div :class="keyboardClass"></div> -->
  <div class="keyboardContainer">
    <div class="simple-keyboard-main"></div>

    <div class="controlArrows">
      <div class="simple-keyboard-control"></div>
      <div class="simple-keyboard-arrows"></div>
    </div>

    <div class="numPad">
      <div class="simple-keyboard-numpad"></div>
      <div class="simple-keyboard-numpadEnd"></div>
    </div>
  </div>
</template>

<script>
import Keyboard from 'simple-keyboard'
import 'simple-keyboard/build/css/index.css'
import layout from 'simple-keyboard-layouts/build/layouts/brazilian'

export default {
  name: 'SimpleKeyboard',
  props: {
    // keyboardClass: {
    //   default: 'simple-keyboard',
    //   type: String
    // },
    input: {
      type: String
    },
    inputName: {
      type: String
    }
  },
  data: () => ({
    keyboard: null,
    keyboardControlPad: null,
    keyboardArrows: null,
    keyboardNumPad: null,
    keyboardNumPadEnd: null
  }),
  mounted() {
    let commonKeyboardOptions = {
      onChange: this.onChange,
      onKeyPress: this.onKeyPress,
      inputName: this.inputName,
      physicalKeyboardHighlight: true,
      physicalKeyboardHighlightPress: true,
      physicalKeyboardHighlightTextColor: 'black',
      physicalKeyboardHighlightBgColor: '#9ab4d0',
      theme: 'simple-keyboard hg-theme-default hg-layout-default',
      syncInstanceInputs: true,
      mergeDisplay: true,
      debug: true
    }
    this.keyboard = new Keyboard('.simple-keyboard-main', {
      ...commonKeyboardOptions,
      ...layout
    })
    this.keyboardControlPad = new Keyboard('.simple-keyboard-control', {
      ...commonKeyboardOptions,
      layout: {
        default: [
          '{prtscr} {scrolllock} {pause}',
          '{insert} {home} {pageup}',
          '{delete} {end} {pagedown}'
        ]
      },
      display: {
        '{escape}': 'esc ⎋',
        '{tab}': 'tab ⇥',
        '{backspace}': 'backspace ⌫',
        '{enter}': 'enter ↵',
        '{capslock}': 'caps lock ⇪',
        '{shiftleft}': 'shift ⇧',
        '{shiftright}': 'shift ⇧',
        '{controlleft}': 'ctrl ⌃',
        '{controlright}': 'ctrl ⌃',
        '{altleft}': 'alt ⌥',
        '{altright}': 'alt ⌥',
        '{metaleft}': 'cmd ⌘',
        '{metaright}': 'cmd ⌘'
      }
    })
    this.keyboardArrows = new Keyboard('.simple-keyboard-arrows', {
      ...commonKeyboardOptions,
      layout: {
        default: ['{arrowup}', '{arrowleft} {arrowdown} {arrowright}']
      }
    })
    this.keyboardNumPad = new Keyboard('.simple-keyboard-numpad', {
      ...commonKeyboardOptions,
      layout: {
        default: [
          '{numlock} {numpaddivide} {numpadmultiply}',
          '{numpad7} {numpad8} {numpad9}',
          '{numpad4} {numpad5} {numpad6}',
          '{numpad1} {numpad2} {numpad3}',
          '{numpad0} {numpaddecimal}'
        ]
      }
    })
    this.keyboardNumPadEnd = new Keyboard('.simple-keyboard-numpadEnd', {
      ...commonKeyboardOptions,
      layout: {
        default: ['{numpadsubtract}', '{numpadadd}', '{numpadenter}']
      }
    })
  },
  methods: {
    onChange(input) {
      this.$emit('onChange', input)
    },
    onKeyPress(button) {
      this.$emit('onKeyPress', button)

      /**
       * If you want to handle the shift and caps lock buttons
       */
      if (button === '{shift}' || button === '{lock}') this.handleShift()
    },
    handleShift() {
      let currentLayout = this.keyboard.options.layoutName
      let shiftToggle = currentLayout === 'default' ? 'shift' : 'default'

      this.keyboard.setOptions({
        layoutName: shiftToggle
      })
    }
  },
  watch: {
    inputName(inputName) {
      console.log('SimpleKeyboard: inputName updated', inputName)
      this.keyboard.setOptions({ inputName })
    },
    input(input) {
      console.log(
        'SimpleKeyboard: input Updated',
        this.keyboard.options.inputName,
        input
      )
      this.keyboard.setInput(input)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
