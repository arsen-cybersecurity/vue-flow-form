<template>
  <span v-bind:data-placeholder="inputType === 'date' ? placeholder : null">
    <the-mask
      v-if="question.mask"
      ref="input"
      v-bind:mask="question.mask"
      v-bind:masked="false"
      v-bind:type="inputType"
      v-bind:value="modelValue"
      v-bind:required="question.required"
      v-on:keydown="onKeyDown"
      v-on:keyup="onChange"
      v-on:focus="setFocus"
      v-on:blur="unsetFocus"
      v-on:keyup.enter.prevent="onEnter"
      v-on:keyup.tab.prevent="onEnter"
      v-bind:placeholder="placeholder"
      v-bind:min="question.min"
      v-bind:max="question.max"
      v-on:change="onChange"
    />
    <input
      v-else
      ref="input"
      :class="[!!error && 'input-error']"
      v-bind:type="inputType"
      v-bind:value="modelValue"
      v-bind:required="question.required"
      v-on:keydown="onKeyDown"
      v-on:keyup="onChange"
      v-on:keyup.enter.prevent="onEnter"
      v-on:keyup.tab.prevent="onEnter"
      v-on:focus="setFocus"
      v-on:blur="unsetFocus"
      v-bind:min="question.min"
      v-bind:max="question.max"
      v-on:input="onChange"
      v-bind:placeholder="placeholder"
      v-bind:maxlength="question.maxLength"
    />
    <span class="text-error text-subtitle-1">{{ error }}</span>
  </span>
</template>

<script>
  /*
    Copyright (c) 2020 - present, DITDOT Ltd. - MIT Licence
    https://github.com/ditdot-dev/vue-flow-form
    https://www.ditdot.hr/en
  */

  import BaseType from './BaseType.vue'
  import { QuestionType } from '../../models/QuestionModel'
  import TheMask from 'vue-the-mask/src/component'

  export default {
    extends: BaseType,
    name: QuestionType.Text,
    inheritAttrs: false,
    components: {
      TheMask
    },

    data() {
      return {
        inputType: 'text',
        canReceiveFocus: true,
        error: '',
      }
    },

    methods: {
      validate() {
        this.error = ''

        if (this.question.mask && this.hasValue) {
          return this.validateMask()
        }

        if (!this.question.required || this.hasValue) {
          const result = this.question.validate(this.dataValue)

          if (typeof result === 'string') {
            this.error = result
          } else {
            return !!result
          }
        } else {
          return false
        }
      },

      validateMask() {
        if (Array.isArray(this.question.mask)) {
          return this.question.mask.some(mask => mask.length === this.dataValue.length)
        }

        return this.dataValue.length === this.question.mask.length
      }
    }
  }
</script>

<style>
.vff .input-error[class] {
border-color: #ff0033;
}

.vff .f-invalid {
  color: #ff0033;
}

</style>