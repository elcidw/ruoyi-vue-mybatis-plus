<template lang="">
  <div v-if="canShow" class="panelWrapper">
    <div class="panelHeader">{{ businessObject.$type }}</div>World
  </div>
</template>
<script>
export default {
  name: 'Panel',
  props: ['element', 'businessObject', 'modeling', 'moddle'],
  data() {
    return {
      canShow: false
    }
  },
  methods: {
    init() {
      this.canShow = true
    }
  }
}
</script>
<style lang="scss">
  .panelWrapper {
  position: absolute;
  right: 0;
  top: 0;
  border-left: 1px solid #ccc;
  height: 100%;
  background-color: #f8f8f8;
  width: 260px;
  .panelHeader {
    padding: 15px;
    padding-bottom: 5px;
    font-size: 120%;
    font-weight: bolder;
  }
  .panelForm {
    padding: 15px;
  }
}
.tableBtn {
  padding: 0;
  font-size: 18px;
}
.title {
  line-height: 32px;
  font-size: 16px;
}
</style>
