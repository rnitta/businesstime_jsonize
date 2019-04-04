<template>
  <div id="timepicker">
    <p>drag to select</p>
    <div class="selectable">
      <DragSelect @change="changeSelection" selectorClass="item">
        <template slot-scope="{ selectedItems }">
          <span :class="selectedN.indexOf(i) > -1 ? 'item active' : 'item'" :data-n="i" v-for="i in Array.from(Array(48).keys())" v-bind:key="i">
            {{
              `${Math.floor(i / 2)
                .toString()
                .padStart(2, '0')}:${((i % 2) * 30).toString().padStart(2, '0')}`
            }}
          </span>
        </template>
      </DragSelect>
    </div>
    <button @click="applyClicked" class="applyButton">apply</button>
  </div>
</template>

<script>
import DragSelect from 'vue-drag-select/src/DragSelect.vue';
export default {
  components: { DragSelect },
  props: {
    applySchedules: Function,
  },
  data: () => ({
    selectedN: [],
  }),
  methods: {
    changeSelection: function(elms) {
      this.selectedN = elms.map(elm => parseInt(elm.dataset.n));
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
.vue-drag-select {
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
  border: 1px solid #eee;
  border-radius: 4px;
}
</style>
