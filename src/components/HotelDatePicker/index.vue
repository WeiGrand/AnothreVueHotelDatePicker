/* eslint no-trailing-spaces: ["error", { "skipBlankLines": true }] */
<template>
  <div
    class="hdp-container"
    v-if="show"
    :style="{left: position.left, top: position.top}"
  >
    <ul></ul>
    <div :style="{width: cols * width + (cols - 1) * 10 + 'px'}">
      <div class="hdp-arrow">
        <em></em>
        <span></span>
      </div>
      <Control
        :index="index"
        :total="total"
        @goPrev="goPrev"
        @goNext="goNext"
      />
      <List
        :ref="`list`"
        :start="start"
        :end="end"
        :cols="data.length > 1 ? cols : 1"
        :format="format"
        :data="data.slice(index, index + 2)"
        @selectCheckDate="selectCheckDate"
        @selectHoverDate="selectHoverDate"
      />
    </div>
  </div>
</template>

<script>
import dayjs from 'dayjs';

import List from './list';
import Control from './control';

export default {
  name: 'AnotherVueHotelDatePicker',
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
      cols: 2, // how many calendar item show each time
      width: 364, // calendar item's width
      checkIn: null, // check in date
      checkOut: null, // check out date
      hoverDate: null, // the dates between check in and the date which mouse is hovering
      index: 0, // current calendar index (the left one)
    };
  },
  computed: {
    data() {
      const {
        start,
        end,
        checkIn,
        checkOut,
        hoverDate,
      } = this;

      const startDate = start ? dayjs(start) : dayjs();
      const endDate = end ? dayjs(end) : dayjs(); // first day of the range
      const first = startDate.startOf('month'); // last day of the range
      const last = endDate.endOf('month');
      const array = []; // render data

      let week = [];
      let month = [];
      let pointer = first; // a pointer for loop
      // loop from the fist day to the last day
      while (pointer.isBefore(last)) {
        let text = null;
        let isHovered = false;
        let isDuring = false;
        const isAvailable = !pointer.isBefore(startDate) && !pointer.isAfter(endDate);

        if (pointer.isSame(checkIn)) {
          text = 'Check In';
        } else if (pointer.isSame(checkOut)) {
          text = 'Check Out';
        }

        if (hoverDate) {
          isHovered = pointer.isAfter(checkIn) && !pointer.isAfter(hoverDate) && isAvailable;
        }

        if (checkOut) {
          isDuring = pointer.isAfter(checkIn) && pointer.isBefore(checkOut);
        }

        week[pointer.day()] = ({
          d: pointer,
          available: isAvailable,
          selected: !!text,
          hovered: isHovered,
          during: isDuring,
          text,
        });

        const weekLength = week.length;

        if (weekLength === 7) {
          month.push(week);
          week = [];
        }

        const newPointer = pointer.add(1, 'day');

        if (newPointer.date() === 1) {
          if (weekLength < 7) {
            week[6] = null;
          }

          month.push(week);
          array.push({
            year: pointer.year(),
            month: pointer.month(),
            data: month,
          });

          week = [];
          month = [];
        }

        pointer = newPointer;
      }

      return array;
    },
    total() {
      return this.data.length || 0;
    },
  },
  methods: {
    success(res) {
      this.$emit('success', res);
    },
    selectCheckDate(date) {
      const {
        checkIn,
        checkOut,
      } = this;

      this.hoverDate = null;

      if (checkIn === null || checkOut !== null) {
        this.checkIn = date;
        this.checkOut = null;

        return;
      }

      this.checkOut = date;

      const {
        format,
      } = this;

      this.$emit('success', {
        checkIn: this.checkIn.format(format),
        checkOut: this.checkOut.format(format),
      });
    },
    selectHoverDate(date) {
      if (this.checkIn === null || this.checkOut !== null) {
        return;
      }

      this.hoverDate = date;
    },
    goPrev() {
      if (this.index === 0) {
        return;
      }

      this.index -= 1;
    },
    goNext() {
      if (this.index + 2 === this.total) {
        return;
      }

      this.index += 1;
    },
  },
  updated() {
    if (!this.hasBindUlEvent) {
      this.$refs.list.$refs.ul.addEventListener('WebkitTransitionEnd', () => {

      });

      this.hasBindUlEvent = true;
    }
  },
  mounted() {
    const el = document.getElementById(this.trigger);
    const { nodeName } = el;
    const event = nodeName === 'INPUT' ? 'focus' : 'click';

    el.addEventListener(event, (e) => {
      const { target } = e;
      const {
        offsetLeft,
        offsetTop,
        offsetHeight,
      } = target;

      this.position.left = `${offsetLeft}px`;
      this.position.top = `${(offsetTop + offsetHeight) - 1}px`;
      this.show = true;

      setTimeout(() => {
        const documentClick = () => {
          this.show = false;

          document.removeEventListener('click', documentClick);
        };

        document.addEventListener('click', documentClick, false);
      }, 0);
    }, false);

    if (nodeName === 'INPUT') {
      el.addEventListener('click', (e) => {
        e.stopPropagation();
      });
    }
  },
};

</script>

<style scoped lang="scss">
  /deep/ * {
    -moz-osx-font-smoothing: grayscale;
    -webkit-font-smoothing: antialiased;
    -webkit-tap-highlight-color: rgba(255, 255, 255, 0);
    -webkit-touch-callout: none;
    -webkit-user-select: none;
    -user-select: none;
  }

  /deep/ div,
  /deep/ h1,
  /deep/ h2,
  /deep/ h3,
  /deep/ h4,
  /deep/ h5,
  /deep/ span,
  /deep/ p,
  /deep/ em,
  /deep/ ul,
  /deep/ li {
    border: 0 none;
    margin: 0;
    padding: 0;
  }

  /deep/ ul {
    list-style: outside none none;
  }

  /deep/ li {
    list-style: none outside none;
  }

  .hdp {

    &-container {
      font-size: 14px;
      color: #333;
      position: absolute;
      -webkit-font-smoothing: antialiased;
      -moz-osx-font-smoothing: grayscale;

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
