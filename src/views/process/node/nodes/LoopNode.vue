<template>
  <div class="node">
    <div class="node-info"  v-if="config.showFooter !== undefined">
      <span>循环组件</span>
      <i 
        class="el-icon-close loop-del-btn"
        title="删除循环组件" 
        @click.stop="$emit('delNode', true)"
      ></i>
    </div>
    <div class="node-error" v-if="config.showFooter !== undefined && showError">
      <el-tooltip effect="dark" :content="errorInfo" placement="top-start">
        <i class="el-icon-warning-outline"></i>
      </el-tooltip>
    </div>
    <div v-if="config.showFooter === undefined || config.showFooter" class="node-footer">
      <div class="btn"  @dragover="handleDragover" @drop="handleDrop">
        <insert-button @insertNode="type => $emit('insertNode', type)"></insert-button>
      </div>
    </div>
  </div>
</template>

<script>
import InsertButton from '@/views/process/node/InsertButton.vue'

export default {
  name: "LoopNode",
  props:{
    config:{
      type: Object,
      default: () => {
        return {}
      }
    }
  },
  components: {InsertButton},
  data() {
    return {
      showError: false,
      errorInfo: '',
    }
  },
  computed:{
    content(){
      // const config = this.config.props
      return '请设置循环组件'
    },
    nodeMap(){
      return this.$store.state.nodeMap
    }
  },
  methods: {
    handleDragover(event) {
      event.preventDefault()
    },
    handleDrop(event) {
      event.preventDefault()
      console.log("🚀 ~ file: InsertButton.vue:74 ~ handleDrop ~ event:", event)
      this.$emit('insertNode', event.dataTransfer.getData("text/plain"))
    },

    getFormItemById(id){
      return this.$store.state.design.formItems.find(item => item.id === id)
    },
    //校验数据配置的合法性
    validate(){
      const parentNode = this.nodeMap.get(this.config.parentId)
      const hasEmpty = parentNode.branchs.every(item => !item.children)
      this.showError = !!hasEmpty
      if(hasEmpty) {
        this.errorInfo = '请设置循环组件'
      }
      return !this.showError
    }
  }
}
</script>

<style lang="less" scoped>
@import "@/assets/theme";

.node{
  padding: 30px 55px 0;
  width: 220px;
  .node-body{
    overflow: hidden;
    cursor: pointer;
    min-height: 80px;
    max-height: 120px;
    position: relative;
    border-radius: 5px;
    background-color: white;
    box-shadow: 0px 0px 5px 0px #d8d8d8;
    &:hover{
      .node-body-main {
        .option{
          display: inline-block !important;
        }
      }
      box-shadow: 0px 0px 3px 0px @primary;
    }
  
    .node-body-main {
      position: absolute;
      width: 188px;
      left: 17px;
      display: inline-block;

      .node-body-main-header{
        padding: 10px 0px 5px;
        font-size: xx-small;
        position: relative;
        .title{
          color: #718dff;
        }
        .option{
          position: absolute;
          right: 0;
          display: none;
          font-size: medium;
          i{
            color: #888888;
            padding: 0 3px;
          }
        }
      }
      .node-body-main-content {
        padding: 6px;
        color: #656363;
        font-size: 14px;

        i {
          position: absolute;
          top: 55%;
          right: 10px;
          font-size: medium;
        }
      }
    }
  }

  .node-footer{
    position: relative;
    .btn{
      width: 100%;
      display: flex;
      height: 70px;
      padding: 20px 0 32px;
      justify-content: center;
    }
    /deep/ .el-button{
      height: 32px;
    }
    &::before{
      content: "";
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      z-index: -1;
      margin: auto;
      width: 2px;
      height: 100%;
      background-color: #CACACA;
    }
  }
  
  .node-info {
    position: absolute;
    top: 0;
    left: 50%;
    padding: 2px 4px;
  }
  .loop-del-btn{
    color: #888888;
    padding: 0 3px;
    cursor: pointer;
    z-index: 2;
    display: none;
  }

  .node-error{
    position: absolute;
    right: 55%;
    top: 40%;
    font-size: 25px;
    color: #F56C6C;
  }
}
</style>
