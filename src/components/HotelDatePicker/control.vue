<template>
  <div class="hdp-control">
    <a :class="{
    'hdp-control-prev': true,
    'is-available': index > 0
    }"
       @click="goPrev"
       href="javascript:;"
    ></a>
    <a :class="{
    'hdp-control-next': true,
    'is-available': index + 2 < total
    }"
       @click="goNext"
       href="javascript:;"
    ></a>
  </div>
</template>

<script>
export default {
  name: 'Control',
  props: {
    index: Number,
    total: Number,
  },
  methods: {
    goPrev() {
      this.$emit('goPrev');
    },
    goNext() {
      this.$emit('goNext');
    },
  },
};
</script>

<style scoped lang="scss">
  .hdp-control {

    >a {
      width: 24px;
      height: 24px;
      border-radius: 2px;
      border: 1px solid #dde0e6;
      position: absolute;
      top: 10px;
      text-align: center;
      font-size: 12px;
      cursor: default;
      background: #fff;
      z-index: 1;
      &:after {
        content: '';
        height: 6px;
        width: 6px;
        border: solid #CCC;
        border-width: 2px 2px 0 0;
        transform: rotate(45deg);
        position: absolute;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
        margin: auto;
      }

      &.is-available {
        cursor: pointer;
        &:hover {
          background: #FAFAFA;
        }
        &:after {
          border-color: #4E7CDD;
        }
      }
    }
    &-prev {
      transform: rotate(180deg);
      left: 10px;
    }
    &-next {
      right: 10px;
    }
  }
</style>
