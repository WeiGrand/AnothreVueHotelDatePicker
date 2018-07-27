<template>
  <div id="app">
    <div class="hotel-date-picker-trigger" id="J-hotel-picker-trigger1">
      Click Here to show the picker
    </div>
    <input type="text" id="J-hotel-picker-trigger" placeholder="Click Here to show the picker">
    <AnotherVueHotelDatePicker
      :trigger="`J-hotel-picker-trigger`"
      :start="`2018-01-02`"
      :end="`2018-12-31`"
      format="YYYY-MM-DD"
      @success="onSuccess"/>
  </div>
</template>

<script>
import AnotherVueHotelDatePicker from './components/HotelDatePicker/';

export default {
  name: 'App',
  components: {
    AnotherVueHotelDatePicker,
  },
  methods: {
    onSuccess(res) {
      window.console.log(res);
    },
  },
};
</script>

<style lang="scss">
  #app {
  padding: 20px;
  position: relative;
}

.hotel-date-picker-trigger {
  width: 346px;
  height: 76px;
  border-radius: 2px;
  background: #FAFAFA;
  border: 1px solid #DDE0E6;
  &:hover {
    border: 1px solid #86ABFF;
  }
}
</style>
