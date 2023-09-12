<template>
  <div :class="{'node': true, 'root': isRoot || !show, 'node-error-state': showError}">
    <div v-if="show" @click="$emit('selected')" :class="{'node-body': true, 'error': showError}" >
      <div>
        <div class="node-body-header" :style="{'background-color': headerBgc}">
          <i :class="headerIcon" style="margin-right: 5px" v-if="(headerIcon || '') !== ''"></i>
          <span>{{title}}</span>
          <i class="el-icon-close" v-if="!isRoot" style="float:right;" @click="$emit('delNode')"></i>
        </div>
        <div class="node-body-content">
          <i :class="leftIcon" v-if="leftIcon"></i>
          <span class="placeholder" v-if="(content || '').trim() === ''">{{placeholder}}</span>
          <ellipsis :row="3" :content="content" v-else/>
          <i class="el-icon-arrow-right"></i>
        </div>
        <div class="node-error" v-if="showError">
          <el-tooltip effect="dark" :content="errorInfo" placement="top-start">
            <i class="el-icon-warning-outline"></i>
          </el-tooltip>
        </div>
      </div>
    </div>
    <div class="node-footer">
      <div class="btn"  @dragover="handleDragover" @drop="handleDrop">
        <insert-button @insertNode="type => $emit('insertNode', type)"></insert-button>
      </div>
    </div>
  </div>
</template>

<script>
import InsertButton from '@/views/process/node/InsertButton.vue'

export default {
  name: "Node",
  components: {InsertButton},
  props:{
    //是否为根节点
    isRoot: {
      type: Boolean,
      default: false
    },
    //是否显示节点体
    show: {
      type: Boolean,
      default: true
    },
    //节点内容区域文字
    content: {
      type: String,
      default: ""
    },
    title:{
      type: String,
      default: "标题"
    },
    placeholder:{
      type: String,
      default: "请设置"
    },
    //节点体左侧图标
    leftIcon: {
      type: String,
      default: undefined
    },
    //头部图标
    headerIcon:{
      type: String,
      default: ''
    },
    //头部背景色
    headerBgc:{
      type: String,
      default: '#576a95'
    },
    //是否显示错误状态
    showError:{
      type: Boolean,
      default: false
    },
    errorInfo:{
      type: String,
      default: '无信息'
    },
  },
  data() {
    return {}
  },
  methods: {
    handleDragover(event) {
      event.preventDefault()
    },
    handleDrop(event) {
      event.preventDefault()
      console.log("🚀 ~ file: InsertButton.vue:74 ~ handleDrop ~ event:", event)
      this.$emit('insertNode', event.dataTransfer.getData("text/plain"))
    }
  }
}
</script>

<style lang="less" scoped>
@import "@/assets/theme";
.root{
  &:before{
    display: none !important;
  }
}
.node-error-state{
  .node-body{
    box-shadow: 0px 0px 5px 0px #F56C6C !important;
  }
}
.node{
  padding: 0 50px;
  width: 220px;
  position: relative;
  &:before{
    content: '';
    position: absolute;
    top: -12px;
    left: 50%;
    -webkit-transform: translateX(-50%);
    transform: translateX(-50%);
    width: 0;
    border-style: solid;
    border-width: 8px 6px 4px;
    border-color: #CACACA transparent transparent;
    background: #F5F5F7;
  }
  .node-body{
    cursor: pointer;
    max-height: 120px;
    position: relative;
    border-radius: 5px;
    background-color: white;
    box-shadow: 0px 0px 5px 0px #d8d8d8;
    &:hover{
      box-shadow: 0px 0px 3px 0px @primary;
      .node-body-header {
        .el-icon-close {
          display: inline;
          font-size: medium;
        }
      }
    }
    .node-body-header{
      border-top-left-radius: 5px;
      border-top-right-radius: 5px;
      padding: 5px 15px;
      color: white;
      font-size: xx-small;
      .el-icon-close{
        display: none;
      }
    }
    .node-body-content{
      padding: 18px;
      color: #656363;
      font-size: 14px;
      i{
        position: absolute;
        top: 55%;
        right: 5px;
        font-size: medium;
      }
      .placeholder{
        color: #8c8c8c;
      }
    }
    .node-error{
      position: absolute;
      right: -40px;
      top: 20px;
      font-size: 25px;
      color: #F56C6C;
    }
  }

  .node-footer{
    position: relative;
    .btn{
      width: 100%;
      display: flex;
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
}
</style>
