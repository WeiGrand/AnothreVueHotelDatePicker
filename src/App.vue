<template>
  <div id="app">
    <div class="hotel-date-picker-trigger" id="J-hotel-picker-trigger">
      Click Here to show the picker
    </div>
    <HotelDatePicker :trigger="`J-hotel-picker-trigger`" :start="`2018-01-02`" :end="`2018-02-01`"/>
  </div>
</template>

<script>
import HotelDatePicker from './components/HotelDatePicker/';

export default {
  name: 'App',
  components: {
    HotelDatePicker,
  },
};
</script>

<style lang="scss">
  * {
    -moz-osx-font-smoothing: grayscale;
    -webkit-font-smoothing: antialiased;
    -webkit-tap-highlight-color: rgba(255, 255, 255, 0);
    -webkit-touch-callout: none;
    -webkit-user-select: none;
    -user-select: none;
  }

  html,
  body,
  div,
  h1,
  h2,
  h3,
  h4,
  h5,
  span,
  p,
  em,
  img,
  dl,
  dt,
  dd,
  ol,
  ul,
  li,
  form,
  label,
  nav,
  footer,
  header,
  section,
  time,
  button,
  input,
  select,
  textarea {
    border: 0 none;
    margin: 0;
    padding: 0;
  }

  h1, h2, h3, h4 {
    font-weight: normal;
  }

  input, textarea {
    border-radius: 0;
    outline: medium none;
    -webkit-user-select: auto;
    user-select: auto;
  }

  input:disabled, textarea:disabled {
    opacity: 1;
  }

  button, input, select, textarea {
    resize: none;
  }

  s, i, em, b, strong{
    font-style: normal;
    text-decoration: none;
  }

  img {
    border: none;
  }

  ul, ol{
    list-style: outside none none;
  }

  li {
    list-style: none outside none;
  }

  button {
    background: transparent;
    outline: 0;
    -moz-osx-font-smoothing: grayscale;
    -webkit-font-smoothing: antialiased;
    -webkit-tap-highlight-color: rgba(255, 255, 255, 0);
    -webkit-touch-callout: none;
    -webkit-user-select: none;
    -user-select: none;
  }

  a {
    text-decoration: none;
  }

  #app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  padding: 20px;
  position: relative;
}

.hotel-date-picker-trigger {
  width: 346px;
  height: 76px;
  border-radius: 2px;
  background: #FAFAFA;
  border: 1px solid #DDE0E6;
  position: absolute;
  &:hover {
    border: 1px solid #86ABFF;
  }
}
</style>
