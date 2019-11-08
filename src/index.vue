<template lang="pug">
  v-menu(offset-y, right, :nudge-bottom="5", :close-on-content-click="false", bottom, v-model="options.dateMenu")
    v-btn.main-btn(round, slot="activator", @click.stop="options.dateMenu = !options.dateMenu")
      span.first-date-range {{ currentStr }}
      span.second-date-range(v-if="options.compare") vs {{ prevStr }}
    v-card
      v-card-text.main-card-text
        v-subheader.text--lighten-1.label-date {{ options.label }}
        v-layout(row)
          v-flex()
            v-layout(row)
              v-layout(row)
                v-flex(xs3)
                  v-subheader.text--lighten-1.label-date {{ options.labelFrom ? options.labelFrom : 'From' }}
                v-flex(xs8)
                  input-picker(:closeOnContent="false", v-model="options.startDate", :min="min", :max="options.endDate || max")
              v-layout(row)
                v-flex(xs3)
                  v-subheader.text--lighten-1.label-date {{ options.labelTo ? options.labelTo : 'To' }}
                v-flex(xs8)
                  input-picker(:closeOnContent="false", v-model="options.endDate", :min="options.startDate || min", :max="max")
            v-layout(style="padding-left: 15px")
              v-checkbox(
                v-model="options.compare",
                v-if="!options.hideCompare",
                :disabled="options.readonly",
                class="compare-checkbox-range",
                color="primary",
                hide-details,
                :label="options.labelCompare ? options.labelCompare : 'Compare'")
            v-layout(row, v-if="options.compare")
              v-layout(row)
                v-flex(xs3)
                  v-subheader.text--lighten-1.label-date {{ options.labelFrom ? options.labelFrom : 'From' }}
                v-flex(xs8)
                  input-picker(:closeOnContent="false", v-model="options.startDatePrev", :min="min", :max="options.startDate || options.endDatePrev || max")
              v-layout(row)
                v-flex(xs3)
                  v-subheader.text--lighten-1.label-date {{ options.labelTo ? options.labelTo : 'To' }}
                v-flex(xs8)
                  input-picker(:closeOnContent="false", v-model="options.endDatePrev", :min="options.startDatePrev || min", :max="options.endDate || max")
          v-flex(style="margin-right: 20px;")
            v-btn(round, center, block, color='primary', @click="onApplyRange(false)", class="button-select-range") Choose
</template>

<script>
  import { format, parse } from 'date-fns'
  import inputPicker from './datePicker.vue'
  export default {
    name: 'v-daterange',
    props: {
      options: {
        type: Object,
        required: true
      }
    },
    components: {
      inputPicker
    },
    data() {
      return {
        dateMenu: false,
        min: null,
        max: null,
        currentStr: this.dateFormat(this.options.startDate) + ' - ' + this.dateFormat(this.options.endDate),
        prevStr: this.dateFormat(this.options.startDatePrev) + ' - ' + this.dateFormat(this.options.endDatePrev)
      }
    },
    methods: {
      onApplyRange(isDownload = false) {
        let obj = {
          current: {
            from: this.options.startDate,
            to: this.options.endDate
          }
        }
        this.currentStr = this.dateFormat(obj.current.from) + ' - ' + this.dateFormat(obj.current.to)
        if (this.options.compare) {
          obj = {
            ...obj,
            prev: {
              from: this.options.startDatePrev,
              to: this.options.endDatePrev
            }
          }
          this.prevStr = this.dateFormat(obj.prev.from) + ' - ' + this.dateFormat(obj.prev.to)
        }
        if (this.options.compare && (obj.prev.from > obj.current.from || obj.prev.to > obj.current.to)) {
          this.$toast.error('Check from date or to date')
        } else {
          this.$emit('onPress', obj)
        }
      },
      dateFormat(dateStr) {
        return format(parse(dateStr), this.options.dateFormat ? this.options.dateFormat : 'DD/MM/YYYY')
      }
    }
  }
</script>

<style lang="stylus">
  .main-btn
    background-color: #22313F !important;
  .main-card-text
    padding: 0px;
  .first-date-range
    color: white;
    font-weight: bold;
  .second-date-range
    color: #B2B2B2;
    font-size: 13px;
    padding-left: 4px;
    text-transform: lowercase;
  .compare-checkbox-range label{
    padding-left: 7px !important;
    font-size: 11px !important;
    padding-bottom: 2px !important;
  }
  .label-date {
    padding-top: 17px !important;
    font-size: 16px !important;
  }
</style>
