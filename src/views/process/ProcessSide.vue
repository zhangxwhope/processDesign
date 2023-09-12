<template>
  <div class="process-side">
    <div class="node-select">
      <div 
        v-for="(item, index) in componentItems"
        :key="index"
        class="select-item"
        draggable="true"
        @dragstart="event => handleDragstart(event, item.type)"
      >
        <i :class="item.icon" :style="`color: ${item.color}`"></i>
        <span>{{ item.name }}</span>
      </div>
    </div>
  </div>
</template>

<script>
import DefaultProps from "./DefaultNodeProps"

export default {
  name: 'ProcessSide',
  data() {
    return {
      componentItems: DefaultProps.COMPONENT_ITEMS
    }
  },
  methods: {
    handleDragstart(event, type) {
      console.log("🚀 ~ file: ProcessSide.vue:30 ~ handleDragstart ~ event:", event)
      event.dataTransfer.setData("text/plain", type)
    }
  }
}
</script>

<style lang="less" scoped>
.process-side{
  width: 200px;
  height: 300px;
  border: 1px solid #ddd;
  overflow: auto;
  .node-select{
    .select-item {
      display: inline-block;
      margin: 5px 5px;
      cursor: pointer;
      padding: 10px 15px;
      border: 1px solid #F8F9F9;
      background-color: #F8F9F9;
      border-radius: 10px;
      width: 130px;
      position: relative;
      span{
        position: absolute;
        left: 65px;
        top: 18px;
      }
      &:hover{
        background-color: #fff;
        box-shadow: 0 0 8px 2px #d6d6d6;
      }
      i{
        font-size: 25px;
        padding: 5px;
        border: 1px solid #dedfdf;
        border-radius: 14px;
      }
    }
  }
}
</style>