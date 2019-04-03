<template>
  <div id="datepicker">
    <VueCal
      @day-focus="focusChanged"
      class="cal vuecal--rounded-theme vuecal--green-theme"
      xsmall
      hide-view-selector
      :time="false"
      default-view="month"
      :disable-views="['week']"
    >
    </VueCal>
  </div>
</template>

<script>
import VueCal from 'vue-cal';
import 'vue-cal/dist/vuecal.css';
import 'material-icons/iconfont/material-icons.scss';
export default {
  components: { VueCal },
  props: {
    msg: String,
  },
  data: () => ({}),
  methods: {
    focusChanged: function(date) {
      this.$parent.mutateDate(date);
    },
  },
};
</script>

<style scoped>
#datepicker {
  display: flex;
  justify-content: center;
  align-items: center;
}
.cal {
  margin: 10px;
  max-width: 300px;
}
</style>
