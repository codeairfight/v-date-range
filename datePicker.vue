<template lang="pug">
  v-menu(lazy, :close-on-content-click='closeOnContent', v-model='menu', transition='scale-transition', offset-y, full-width, :nudge-right='0', max-width='460px', min-width='460px', :disabled="disabled")
    v-text-field(slot='activator',
    single-line,
    v-model='showDate',
    append-icon='fa-calendar-alt',
    :disabled="disabled",
    readonly,
    :error-messages="errorMessages"
    @blur="inputDate = parseDate(showDate)")
    v-date-picker(v-model='inputDate', :min="min", :max="max", scrollable, landscape, @input="showDate = formatDate($event)", :allowed-dates="allowedDates", :disabled="disabled")
</template>

<script>

export default {
  props: {
    value: {
      default: null,
      required: true
    },
    allowedDates: {
      default: null
    },
    errorMessages: {
      default: function () {
        return []
      }
    },
    closeOnContent: {
      default: false
    },
    disabled: {
      type: Boolean,
      default: false
    },
    min: {
      default: null
    },
    max: {
      default: null
    }
  },

  data() {
    return {
      menu: false,
      showDate: null
    }
  },

  computed: {
    inputDate: {
      get: function () {
        this.showDate = this.formatDate(this.value)
        return this.value
      },
      set: function (newValue) {
        this.$emit('input', newValue)
        return newValue
      }
    }
  },

  methods: {
    formatDate(date) {
      if (!date) {
        return null
      }
      const [year, month, day] = date.split('-')
      return `${day}/${month}/${year}`
    },
    parseDate(date) {
      if (!date) {
        return null
      }
      const [day, month, year] = date.split('/')
      return `${year}-${month.padStart(2, '0')}-${day.padStart(2, '0')}`
    }
  }
}
</script>
