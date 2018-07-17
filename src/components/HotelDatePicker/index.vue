/* eslint no-trailing-spaces: ["error", { "skipBlankLines": true }] */
<template>
  <div
    class="hdp-container"
    v-if="show"
    :style="{left: position.left, top: position.top}"
  >
    <div :style="{width: cols * width + 'px'}">
      <div class="hdp-arrow">
        <em/>
        <span/>
      </div>
      <Control />
      <List
        :start="start"
        :end="end"
        :cols="cols"
        :format="format"
        @success="success"
      />
    </div>
  </div>
</template>

<script>
import List from './list';
import Control from './control';

export default {
  name: 'HotelDatePicker',
  components: {
    List,
    Control,
  },
  props: {
    trigger: String, // element's id,
    start: [String, Object],
    end: [String, Object],
    format: String,
  },
  data() {
    return {
      show: false,
      position: {
        left: 'auto',
        top: 'auto',
      },
      cols: 2,
      width: 365,
    };
  },
  methods: {
    success(res) {
      this.$emit('success', res);
    },
  },
  mounted() {
    const el = document.getElementById(this.trigger);
    el.addEventListener('click', (e) => {
      const { target } = e;
      const {
        offsetLeft,
        offsetTop,
        offsetHeight,
      } = target;

      this.position.left = `${offsetLeft}px`;
      this.position.top = `${(offsetTop + offsetHeight) - 1}px`;
      this.show = true;
    }, false);
  },
};
</script>

<style scoped lang="scss">
  .hdp {
    &-container {
      font-size: 14px;
      color: #333;
      position: absolute;
      >div {
        background: #FFF;
        padding: 10px;
        border: 1px solid #DDE0E6;
        box-shadow: 0 4px 10px 0 rgba(0, 0, 0, .05);
        position: relative;
      }
    }
    &-arrow {
      position: absolute;
      left: 20px;
      top: -4px;
      z-index: 9;
      em,
      span {
        position: absolute;
        width: 0;
        height: 0;
        border: solid transparent;
        overflow: hidden;
        top: 0;
        left: 0;
      }
      em {
        border-width: 0 4px 4px;
        top: -1px;
        left: 0;
        border-bottom-color: #dde0e6;
      }
      span {
        border-width: 0 4px 4px;
        border-bottom-color: #fff;
      }
    }
  }
</style>
