<template>
  <li class="hdp-item">
    <div class="hdp-item-hd">
      {{monthName[data.month]}} {{data.year}}
    </div>
    <table class="hdp-item-bd">
      <tbody>
        <tr v-for="(week, index) in data.data" :key="index">
          <td v-for="(day, index) in week"
              v-if="day"
              :class="{'is-available': day.available}"
              :key="index">
            <h4>{{day.d.date()}}</h4>
          </td>
          <td v-else></td>
        </tr>
      </tbody>
    </table>
  </li>
</template>

<script>
export default {
  name: 'Item',
  props: {
    data: Object,
  },
  data() {
    return {
      monthName: ['January', 'February', 'March', 'April', 'May', 'June', 'July', 'August', 'September', 'October', 'November', 'December'],
    };
  },
};
</script>

<style scoped lang="scss">
.hdp-item {
  width: 362px;
  display: inline-block;
  vertical-align: top;
  margin-right: 10px;
  white-space: normal;
  &-hd {
    font-size: 16px;
    font-weight: bold;
    line-height: 1;
    padding-bottom: 38px;
    text-align: center;
  }
  &-bd {
    border-collapse: collapse;
    border-spacing: 0;
    width: 100%;
    table-layout: fixed;

    td {
      width: 52px;
      height: 52px;
      box-sizing: border-box;
      border: 1px solid #ebebeb;
      text-align: center;

      h4 {
        line-height: 1;
        color: #ccc;
      }

      h5 {
        font-size: 12px;
        line-height: 1;
        margin-top: 4px;
      }

      &.is-available {
        cursor: pointer;
        h4 {
          font-weight: bold;
          color: #333;
        }
        &:hover {
          background: #FAFAFA;
        }
      }

      &.is-selected {
        background: #4E7CDD;
        border-color: #4E7CDD;
        h4,
        h5 {
          color: #FFF;
        }
        &:hover {
          background: #A7BEEE;
          border-color: #A7BEEE;
        }
      }
    }
  }
}
</style>
