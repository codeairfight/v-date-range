Date Range Package for VuetifyJS
## Installation:
npm i v-date-range
## Usage:
``` js
import VDateRange from 'v-date-range';
```
Add to components
``` js
...
components: {
  VDateRange
}
...

```
Add to template
``` js
v-date-range(:options="VDateRangeOptions", @onPress="onPress")
```
Add onPress function to methods
``` js
...
methods: {
  onPress(dateRange) {
    console.log('result choose', dateRange)
  }
}
...
```
options parameter:
``` js
VDateRangeOptions: {
  startDate: "", // start date default
  endDate: "", // end date default
  minDate: "", // min date validate
  maxDate: "", // max date validate
  startDatePrev: "", // previous start date default
  endDatePrev: "", // previous end date default
  maxDatePrev: "", // previous min date validate
  minDatePrev: "", // previous max date validate
  readonly: true | false, // disable action
  compare: true | false, // compare enable
  format: 'DD/MM/YYYY', // date format
  dateMenu: false, // menu control
  labelFrom: 'From', // from label
  labelTo: 'To', // to label,
  labelCompare: 'Compare', // compare label
  hideCompare: true | false // hide second date
}
```
