<template>
  <div class="hdp-list">
    <div class="hdp-list-weekday">
      <ul>
        <li v-for="(day, index) in weekday"
            :key="index"
            :class="{'is-weekend': index === 0 || index === 6}">{{day}}</li>
      </ul>
      <ul v-if="cols > 1">
        <li v-for="(day, index) in weekday"
            :key="index"
            :class="{'is-weekend': index === 0 || index === 6}">{{day}}</li>
      </ul>
    </div>
    <ul>
      <Item v-for="(month, index) in data"
            :key="index"
            :data="month"
            :checkIn="checkIn"
            :checkOut="checkOut"
            @selectCheckDate="selectCheckDate"
            @selectHoverDate="selectHoverDate"
      />
    </ul>
  </div>
</template>

<script>
import dayjs from 'dayjs';
import Item from './item';

export default {
  name: 'List',
  components: {
    Item,
  },
  props: {
    start: [String, Object],
    end: [String, Object],
    cols: Number,
    format: String,
  },
  data() {
    return {
      weekday: ['Sun', 'Mon', 'Tue', 'Wed', 'Thur', 'Fri', 'Sat'],
      checkIn: null,
      checkOut: null,
      hoverDate: null,
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
  },
  methods: {
    selectCheckDate(date) {
      const {
        checkIn,
        checkOut,
      } = this;

      this.hoverDate = null;

      if (checkIn === null || checkOut !== null || date.isBefore(checkIn)) {
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
  },
  mounted() {},
};
</script>

<style scoped lang="scss">
  .hdp-list {
    overflow: hidden;
    &-weekday {
      position: absolute;
      left: 0;
      top: 43px;
      overflow: hidden;
      >ul {
        float: left;
        overflow: hidden;
        margin-left: 10px;
        li {
          font-size: 12px;
          line-height: 18px;
          width: 52px;
          float: left;
          text-align: center;
          color: #666;
          &.is-weekend {
            color: #FF5A82;
          }
        }
      }
    }
    >ul {
      white-space: nowrap;
    }
  }
</style>
