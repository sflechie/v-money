<template lang="html">
  <input type="tel"
         :value="formattedValue"
         @change="change"
         v-money="{precision, decimal, thousands, prefix, suffix, allowBlank, min, max}"
         :class="className"
         :placeholder="placeholder"
         :id="id"
         :maxlength="maxlength"
         @blur="$emit('blur')" />
</template>

<script>
import money from './directive'
import defaults from './options'
import {format, unformat} from './utils'

export default {
  name: 'Money',
  props: {
    value: {
      required: true,
      type: [Number, String],
      default: 0
    },
    class: {
      type: String,
      default: 'v-money'
    },
    placeholder: {
      type: String,
      default: ''
    },
    id: {
      type: String
    },
    maxlength: {
      type: Number
    },
    masked: {
      type: Boolean,
      default: false
    },
    precision: {
      type: Number,
      default: () => defaults.precision
    },
    decimal: {
      type: String,
      default: () => defaults.decimal
    },
    thousands: {
      type: String,
      default: () => defaults.thousands
    },
    prefix: {
      type: String,
      default: () => defaults.prefix
    },
    suffix: {
      type: String,
      default: () => defaults.suffix
    },
    allowBlank: {
      type: Boolean,
      default: () => defaults.allowBlank
    },
    max: {
      type: Number,
      default: () => defaults.max
    },
    min: {
      type: Number,
      default: () => defaults.min
    }
  },

  directives: {money},

  computed: {
    className() {
      return this.class;
    }
  },

  data () {
    return {
      formattedValue: ''
    }
  },

  watch: {
    value: {
      immediate: true,
      handler (newValue, oldValue) {
        var formatted = format(newValue, this.$props)
        if (formatted !== this.formattedValue) {
          this.formattedValue = formatted
        }
      }
    }
  },

  methods: {
    change (evt) {
      this.$emit('input', this.masked ? evt.target.value : unformat(evt.target.value, this.precision))
    }
  }
}
</script>
