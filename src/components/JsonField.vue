<template>
  <div id="jsonfield">
    <p class="error"></p>
    <textarea name="" id="" v-model="content"></textarea>
  </div>
</template>

<script>
  export default {
    props: {
      // content: Object
    },
    data: () => ({
      content: '{}'

    }),
    methods: {
      mutateContent: function(newContent) {
        this.content = newContent
      }

    },
    computed: {
      // contentstr: function(){
      //   return JSON.stringify(this.content);
      // }
    }
  };
</script>

<style scoped lang="scss">
  #jsonfield {
    display: flex;
    height: 100%;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
p.error{
  text-align: center;
}
p.error.hidden {
  display: none;
}
  textarea {
    padding: 20px;
    width: 100%;
    flex-grow: 1;
    overflow: scroll;
  }
</style>
