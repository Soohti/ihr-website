<template>
  <q-icon name="fas fa-calendar-day" class="cursor-pointer" :class="textColor">
    <q-popup-proxy id="popupid" v-model="show">
      <q-date :value="qTimeModel" @input="propagate($event)" :mask="mask" :options="options" color="accent" />
    </q-popup-proxy>
  </q-icon>
</template>
<script>
const QTIME_MASK = 'YYYY-MM-DDTHH:mm:ss.SSSZ'

export default {
  props: {
    value: {
      type: Date,
      required: true,
    },
    min: {
      type: Date,
      required: true,
      default: () => {
        return new Date()
      },
    },
    max: {
      type: Date,
      required: true,
      default: () => {
        return new Date()
      },
    },
    white: {
      type: Boolean,
    },
    hideTime: {
      type: Boolean,
      default: false,
    },
  },
  emits: {
    'input': function(selectedDate) {
      if (selectedDate !== null) {
        return true;
      } else {
        console.warn('SelectedDate is missing!');
        return false;
      }
    }
  },
  data() {
    // console.log(this.value.toISOString())
    return {
      myId: `dateTimePicker${this._uid}`,
      selectedDateTime: this.$options.filters.ihrUtcString(this.value),
      mask: QTIME_MASK,
      qTimeModel: this.value.toISOString(),
      show: false,
    }
  },
  methods: {
    options(selectDate) {
      selectDate = new Date(selectDate)
      return selectDate >= this.min && selectDate <= this.max
    },
    propagate(event) {
      if(event === null) return;
      let selectedDate = new Date(event)
      this.qTimeModel = selectedDate.toISOString()
      ;(this.selectedDateTime = this.$options.filters.ihrUtcString(selectedDate)), (this.show = false)
      this.$emit('input', selectedDate)
    },
  },
  computed: {
    textColor() {
      return this.white ? 'IHR_white-text' : 'IHR_black-text'
    },
  },
}
</script>

<style lang="stylus" scoped>
.IHR_
  &date-time-picker
    cursor pointer

  &white-text
    color white

  &black-text
    color black

  &date-input
    font-weight bolder
</style>
