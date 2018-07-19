# Another Vue Hotel Date Picker

A very simple but useful hotel date picker for `Vue.js`

## Installation

```bash
npm install another-vue-hotel-date-picker --save
```

## Usage

```javascript
import vueHotelDatePicker from 'another-vue-hotel-date-picker';

export default {
  components: {
    vueHotelDatePicker,
  },
}
```

```javascript
<vueHotelDatePicker
      :trigger="`J-hotel-picker-trigger`"
      :start="`2018-01-02`"
      :end="`2018-12-31`"
      format="YYYY-MM-DD"
      @success="onSuccess"/>
```
## Props

### trigger
- Type: `String`
- Required: `true`,
- Default: `undefined`

The Element's id for `getElementById` which will trigger the picker

### start
- Type: `Date` or `String` or `dayjs Object`
- Required: `true`,
- Default: `undefined`

The start view date.

### end
- Type: `Date` or `String` or `dayjs Object`
- Required: `true`,
- Default: `undefined`

The start end date.

### format
- Type: `String`'
- Required: `false`,
- Default: `undefined`

The date format string.

### success
- Type: `Function`'
- Required: `false`,
- Default: `undefined`

The callback function when user selected both the check in and check out date
