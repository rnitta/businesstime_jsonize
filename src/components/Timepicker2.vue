<template>
  <div id="timepicker">
    <p>click to select</p>
    <div class="selectables">
      <span :class="selectedN.indexOf(i) > -1 ? 'item active' : 'item'" :data-n="i" v-for="i in Array.from(Array(48).keys())" v-bind:key="i" @click="slectClick">
        {{
          `${Math.floor(i / 2)
            .toString()
            .padStart(2, '0')}:${((i % 2) * 30).toString().padStart(2, '0')}`
        }}
      </span>
    </div>
    <button @click="applyClicked" class="applyButton">apply</button>
  </div>
</template>

<script>
export default {
  props: {
    applySchedules: Function,
  },
  data: () => ({
    selectedN: [],
  }),
  methods: {
    slectClick: function(e) {
      const clickedN = parseInt(e.target.dataset.n);
      if (this.selectedN.indexOf(clickedN) > -1) {
        this.selectedN = this.selectedN.filter(n => n !== clickedN);
      } else {
        this.selectedN.push(clickedN)
      }
    },
    // これはひどい
    reloadSelection: function(newSelectedN) {
      this.selectedN = newSelectedN.map(v => {
        const c = v.split(':').map(str => parseInt(str));
        return c[0] * 2 + c[1] / 30;
      });
    },
    applyClicked: function() {
      this.$parent.addSchedules(
        this.selectedN.map(
          i =>
            `${Math.floor(i / 2)
              .toString()
              .padStart(2, '0')}:${((i % 2) * 30).toString().padStart(2, '0')}`
        )
      );
    },
  },
};
</script>

<style scoped>
#timepicker {
  display: flex;
  height: 100%;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.selectables {
  display: flex;
  align-items: center;
  flex-wrap: wrap;
  width: 100%;
  flex: 1;
}

.item {
  display: block;
  padding: 10px 0;
  min-width: calc(100% / 13);
  margin: 2px;
  border: 1px dashed #eee;
}
.item.active {
  background-color: lightcyan;
}
.applyButton {
  margin: 10px 0;
  padding: 10px;
  border: 1px solid #666;
  border-radius: 4px;
}
</style>
