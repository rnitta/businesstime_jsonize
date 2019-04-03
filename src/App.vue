<template>
  <div id="app">
    <div class="upper">
      <Datepicker />
      <Timepicker ref="timepicker" />
    </div>
    <div class="lower">
      <JsonField :content="result" ref="jsonfield" />
    </div>
  </div>
</template>

<script>
import Datepicker from './components/Datepicker';
import Timepicker from './components/Timepicker';
import JsonField from './components/JsonField';

export default {
  name: 'app',
  components: {
    JsonField,
    Datepicker,
    Timepicker,
  },
  //fixme: 型アノテーション書く
  data: () => ({
    result: {},
    focusedDate: new Date(),
  }),
  computed: {
    formattedFocusedDate: function() {
      const y = this.focusedDate
        .getFullYear()
        .toString()
        .padStart(2, '0');
      const m = (this.focusedDate.getMonth() + 1).toString().padStart(2, '0');
      const d = this.focusedDate
        .getDate()
        .toString()
        .padStart(2, '0');
      return `${y}-${m}-${d}`;
    },
    focusedWeekday: function() {
      return [
        'sunday',
        'monday',
        'tuesday',
        'wednesday',
        'thursday',
        'friday',
        'saturday'
      ][this.focusedDate.getDay()]
    }
  },
  methods: {
    //fixme: 流石に適当すぎ
    mutateDate: function(date) {
      this.focusedDate = date;
      this.$refs.timepicker.reloadSelection((this.result[this.formattedFocusedDate] || {})[this.focusedWeekday] || []);
    },
    // dates: [Array<String>]
    addSchedules: function(dates) {
      // 追加するのみなら
      //const prev = (this.result[this.formattedFocusedDate] || {})[this.focusedWeekday] || [];
      //this.result[this.formattedFocusedDate] = {[this.focusedWeekday]: [...prev, ...dates].filter((x, i, self) => self.indexOf(x) === i).sort()};
      //上書きする
      this.result[this.formattedFocusedDate] = {[this.focusedWeekday]: [...dates].filter((x, i, self) => self.indexOf(x) === i).sort()};
      this.updateJson();
    },
    updateJson: function() {
      this.$refs.jsonfield.mutateContent(JSON.stringify(this.result))
    },
  },
};
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  display: flex;
  flex-direction: column;

  & > * {
    height: 50vh;
  }
}

.upper {
  display: flex;
  align-items: center;
  border-bottom: 1px solid #eee;
}

.upper > *:first-child {
  width: 30vw;
}
.upper > *:last-child {
  width: 70vw;
}
</style>
