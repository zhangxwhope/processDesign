<template>
  <el-popover placement="bottom-start" title="添加流程节点" width="350" trigger="click">
    <div class="node-select">
      <div @click="addAPI">
        <i class="el-icon-message" style="color:rgb(255, 148, 62);"></i>
        <span>请求节点</span>
      </div>
      <div @click="addDate">
        <i class="el-icon-date" style="color:rgb(50, 150, 250);"></i>
        <span>日期组件</span>
      </div>
      <div @click="addDynamicScript">
        <i class="el-icon-c-scale-to-original" style="color:rgb(21, 188, 131);"></i>
        <span>动态脚本</span>
      </div>
      <div @click="addString">
        <i class="el-icon-help" style="color:#718dff;"></i>
        <span>字符串组件</span>
      </div>
      <div @click="addXml">
        <i class="el-icon-folder-delete" style="color:#f25643;"></i>
        <span>xml组件</span>
      </div>
      <div @click="addJson">
        <i class="el-icon-document" style="color:#c9bc0c;"></i>
        <span>json组件</span>
      </div>
      <div @click="addConcurrents">
        <i class="el-icon-share" style="color:#3c2fe3;"></i>
        <span>分支组件</span>
      </div>
      <div @click="addVariable">
        <i class="el-icon-position" style="color:#f314e0;"></i>
        <span>变量组件</span>
      </div>
      <div @click="addLoop">
        <i class="el-icon-refresh" style="color:#9896a7;"></i>
        <span>循环组件</span>
      </div>
      <div @click="addDataConversion">
        <i class="el-icon-s-operation" style="color:#85d707;"></i>
        <span>数据转换</span>
      </div>
    </div>
    <el-button icon="el-icon-plus" slot="reference" type="primary" size="small" circle></el-button>
  </el-popover>
</template>

<script>
export default {
  name: "InsertButton",
  components: {},
  data() {
    return {}
  },
  computed:{
    selectedNode(){
      return this.$store.state.selectedNode
    }
  },
  methods: {
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
