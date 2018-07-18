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
    <ul :ref="`ul`">
      <Item v-for="(month, index) in data"
            :key="index"
            :data="month"
            @selectCheckDate="selectCheckDate"
            @selectHoverDate="selectHoverDate"
      />
    </ul>
  </div>
</template>

<script>
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
    checkIn: Object,
    checkOut: Object,
    hoverDate: Object,
    data: Array,
  },
  data() {
    return {
      weekday: ['Sun', 'Mon', 'Tue', 'Wed', 'Thur', 'Fri', 'Sat'],
    };
  },
  methods: {
    selectCheckDate(date) {
      this.$emit('selectCheckDate', date);
    },
    selectHoverDate(date) {
      this.$emit('selectHoverDate', date);
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
