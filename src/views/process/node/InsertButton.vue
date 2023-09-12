<template>
  <el-popover placement="bottom-start" title="添加流程节点" width="350" trigger="click">
    <div class="node-select">
      <div
        v-for="(item, index) in componentItems"
        :key="index"
        class="select-item"
        @click="handleClick(item.type)"
      >
        <i :class="item.icon" :style="`color: ${item.color}`"></i>
        <span>{{ item.name }}</span>
      </div>
    </div>
    <el-button icon="el-icon-plus" slot="reference" type="primary" size="small" circle></el-button>
  </el-popover>
</template>

<script>
import DefaultProps from "../DefaultNodeProps"

export default {
  name: "InsertButton",
  components: {},
  data() {
    return {
      componentItems: DefaultProps.COMPONENT_ITEMS
    }
  },
  computed:{
    selectedNode(){
      return this.$store.state.selectedNode
    }
  },
  methods: {
    handleClick(type) {
      this.$emit('insertNode', type)
    },
    addAPI() {
      this.$emit('insertNode', "API")
    },
    addDate() {
      this.$emit('insertNode', "TIMER")
    },
    addDynamicScript() {
      this.$emit('insertNode', "DYNAMIC")
    },
    addString() {
      this.$emit('insertNode', "STRING")
    },
    addXml() {
      this.$emit('insertNode', "XML")
    },
    addJson() {
      this.$emit('insertNode', "JSON")
    },
    addConcurrents() {
      this.$emit('insertNode', "CONCURRENTS")
    },
    addVariable() {
      this.$emit('insertNode', "VARIABLE")
    },
    addLoop() {
      this.$emit('insertNode', "LOOP")
    },
    addDataConversion() {
      this.$emit('insertNode', "CONVERSION")
    },

    addApprovalNode(){
      this.$emit('insertNode', "APPROVAL")
    },
    addCcNode(){
      this.$emit('insertNode', "CC")
    },
    addDelayNode(){
      this.$emit('insertNode', "DELAY")
    },
    addConditionsNode(){
      this.$emit('insertNode', "CONDITIONS")
    },
    addConcurrentsNode(){
      this.$emit('insertNode', "CONCURRENTS")
    },
    addTriggerNode(){
      this.$emit('insertNode', "TRIGGER")
    }
  }
}
</script>

<style lang="less" scoped>
.node-select{
  div{
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
</style>
